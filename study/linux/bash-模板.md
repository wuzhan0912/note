```bash
#!/bin/bash
set -o pipefail
set -u

## default env config
LOG_ROOT_PATH="."
LOG_FILE_NAME="app.log"
EXTERNAL_LOADER_PATH="../"

APP_BIN=`pwd`
CONF_DIR="${APP_BIN}/../conf"
APM_DIR="${APP_BIN}/../apm/apm-agent.jar"

#Time Config
START_WAIT=5
SHUT_WAIT=10
KILL_INTERVAL=5
KILL_TIMES=5

SHUTDOWN_HOST=127.0.0.1

JAVA_APP_OPTS=""
source ${APP_BIN}/env.bash

if [[ "${PROFILES}" == "docker" ]];then
    APP_MIN_MEMORY=512
    APP_MAX_MEMORY=1500
    echo ${NOAH_ENV}

    if [[ "${NOAH_ENV}" == "ga" ]] ;then
        APP_MIN_MEMORY= ${APP_MAX_MEMORY}
    fi
    JAVA_OPTS="-Xms${APP_MIN_MEMORY}m -Xmx${APP_MAX_MEMORY}m -Xss256k -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=256m -XX:NewRatio=2 -XX:SurvivorRatio=6"
    SERVER_NAME=${NOAH_APPNAME}
fi

#evironment set
JAVA_OPTS="${JAVA_OPTS} -XX:MaxTenuringThreshold=10"
JAVA_OPTS="${JAVA_OPTS} -XX:+PrintGCDetails -XX:+PrintGCTimeStamps"
JAVA_OPTS="${JAVA_OPTS} -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=${LOG_ROOT_PATH}"
JAVA_OPTS="${JAVA_OPTS} -javaagent:${APM_DIR}"

JAVA_OPTS="${JAVA_OPTS} -Dspring.profiles.active=${PROFILES}"
JAVA_OPTS="${JAVA_OPTS} -Dspring.application.name=${SERVER_NAME} -Dserver.port=${SERVER_PORT}"
JAVA_OPTS="${JAVA_OPTS} -Dloader.path=${APP_BIN}/../conf,${EXTERNAL_LOADER_PATH}"
JAVA_OPTS="${JAVA_OPTS} -Dlogging.path=${LOG_ROOT_PATH} -Dendpoints.logfile.external-file=${LOG_ROOT_PATH}/${LOG_FILE_NAME}"
JAVA_OPTS="${JAVA_OPTS} -Dskywalking_ext.agent.port=${SERVER_PORT} -Dskywalking.agent.application_code=${SERVER_NAME} -Djava.net.preferIPv4Stack=true"
JAVA_OPTS="${JAVA_OPTS} ${JAVA_APP_OPTS}"

#start_jar=""
#if [[ -z START_JAR ]];then
#    start_jar=${START_JAR}
#else
#  ## default jars
#    start_jar=`find .. -name "*.jar" | xargs`
#fi

CLASSPATH=${CONF_DIR}
START_UP_EXEC="java ${JAVA_OPTS} -server -cp ${CLASSPATH} -jar ${APP_BIN}/../${START_CLASS} >> ${EXEC_STD_OUT}"

#RETURN CODES
RET_SUCCESS=0
RET_INSTANCE_DEAD=1
RET_ERROR_SHUT=2
RET_ERROR_START=3
RET_STATUS_ALIVE=0
RET_STATUS_NOT_ALIVE=1
INSTANCE_PID=app.pid

function get_Pid(){
    if [[ ! -z ${INSTANCE_PID} ]] && [[ -f ${INSTANCE_PID} ]];then
        cat "${INSTANCE_PID}"
    else
        ps -ef | grep -vE "grep $APP_BIN|$0" | grep ${APP_BIN} | grep ${START_CLASS} | awk '{print $2}'
    fi
}

#get pid
PID=`get_Pid`

function usage(){
        cat <<EOM
    Purpose  :    This script encapsulates the spring boot jar and just acts like a controller.
    Usage    :    bash ${0} start|shutdown|kill|force|restart|status
    Date     :    2017.03
EOM
}

#test instance alived or not
function is_Instance_Alive(){
    #kill -0 : test process alived or not
    if `kill -0 ${PID} 2>/dev/null` ; then
    #0 stands for success in shell
        return ${RET_SUCCESS}
    elif is_Instance_Up;then
        return ${RET_SUCCESS}
    else
        return ${RET_INSTANCE_DEAD}
    fi
}

# 检测是否启动
function is_Instance_Up(){
    status_code=`curl -o /dev/null -s -w %{http_code} "http://${SHUTDOWN_HOST}:${SERVER_PORT}/actuator/health"`
    if [[ ${status_code} = 200 ]]; then
        return ${RET_SUCCESS}
    else
        return ${RET_INSTANCE_DEAD}
    fi
}

## remove pid file
function remove_Pid(){
   if [ ! -z ${INSTANCE_PID} ];then
       if [ -f ${INSTANCE_PID} ];then
           rm -f ${INSTANCE_PID}
       fi
   fi
}

## shutdown Instance
function shutdown_Instance(){
    if ! is_Instance_Alive ;then
        echo "no need to stop, not found PID"
        return ${RET_SUCCESS}
    else
        echo -n "shutdown instance"
        for((i=0;i<=$KILL_TIMES;i++ ));do   #kill instance for KILL_TIMES, each with KILL_INTERVAL secs
            curl -d "" "http://${SHUTDOWN_HOST}:${SERVER_PORT}/actuator/shutdown"
            sleep_Wait ${SHUT_WAIT}
            #check if kill of this round success
            if ! is_Instance_Alive ; then
                remove_Pid
                echo shutdown successfully
                return ${RET_SUCCESS}
            fi
        done
        return ${RET_ERROR_SHUT}
    fi
}

#normal kill
function kill_Instance(){
    if ! is_Instance_Alive ;then
        echo "no need to stop, not found PID"
        return ${RET_SUCCESS}
    else
        echo -n "killing $PID "
    for((i=0;i<=$KILL_TIMES;i++ ));do    #kill instance for KILL_TIMES, each with KILL_INTERVAL secs
        kill -15 ${PID}
        sleep_Wait ${KILL_INTERVAL}
        #check if kill of this round success
        if ! is_Instance_Alive ; then
            remove_Pid
            echo kill successfully
            return ${RET_SUCCESS}
        fi
    done
    return ${RET_ERROR_SHUT}
    fi
}
#-9 kill
function force_Instance(){
    if ! is_Instance_Alive ;then
        echo "no need to force kill, not found PID"
        return ${RET_SUCCESS}
    else
    echo -n "force killing $PID "
    for((i=0;i<=$KILL_TIMES;i++ ));do
        #kill instance for KILL_TIMES, each with KILL_INTERVAL secs
        kill -9 ${PID}
        sleep_Wait ${KILL_INTERVAL}
        if ! is_Instance_Alive ; then
            remove_Pid
            echo force kill successfully    #check if force kill of this round success
            return ${RET_SUCCESS}
        fi
    done
    echo shutdown failed, please manually check
    return ${RET_ERROR_SHUT}
    fi
}

#wait
function sleep_Wait(){
    local sec=$1
    for((i=1;i<=$sec;i++ ));do
        sleep 1
        echo -n "."
    done
    echo
}

#start up
function startup_Instance(){
##This function startup the instance
    if is_Instance_Alive; then
        echo no need to start, instance pid : ${PID}, exit
    else
        echo exec ${START_UP_EXEC}
        echo -n "starting "
        nohup ${START_UP_EXEC} > ${EXEC_STD_OUT} 2>&1 &
        if [ ! -z ${INSTANCE_PID} ];then
            echo $! > "${INSTANCE_PID}"
        fi
        sleep_Wait ${START_WAIT}
        if ! is_Instance_Alive ; then
            echo start successfully : `get_Pid`
            return ${RET_SUCCESS}
        else
            echo start failed
            return ${RET_ERROR_START}
        fi
    fi
}

#start up
function startup_Instance_Without_Nohup(){
##This function startup the instance
    if is_Instance_Alive; then
        echo no need to start, instance pid : ${PID}, exit
    else
        echo exec ${START_UP_EXEC}
        echo -n "starting "
        ${START_UP_EXEC}
    fi
}

#check instance status
function status_Instance(){
    if is_Instance_Alive;then
        echo "Instance is Alived, pid:${PID}"
        return ${RET_STATUS_ALIVE}
    else
        echo "Instance is not Alived"
        return ${RET_STATUS_NOT_ALIVE}
    fi
}

###
function check_Health_Instance(){
    if is_Instance_Up;then
        echo "Instance is Up"
        return ${RET_STATUS_ALIVE}
    else
        echo "Instance is not Alived"
        return ${RET_STATUS_NOT_ALIVE}
    fi
}

#dispatcher
function dispatcher(){
    if [ $# -lt 1 ] ;then
        usage
        exit -1
    fi
    if [ -z ${PROFILES} ];then
        echo "Please specific profiles before excute this script, example: export PROFILES=dev"
        exit -1
    fi

    local args=$1
    case "$args" in
    kill)
        kill_Instance
        ;;
    force)
        force_Instance
        ;;
    start)
        startup_Instance
        ;;
    fstart)
        startup_Instance_Without_Nohup
        ;;
    shutdown)
        shutdown_Instance
        ;;
    restart)
        shutdown_Instance && startup_Instance
        ;;
    status)
        status_Instance
        ;;
    health)
        check_Health_Instance
        ;;
    *)
        usage
        ;;
    esac
}

dispatcher "$@"
#END
```

