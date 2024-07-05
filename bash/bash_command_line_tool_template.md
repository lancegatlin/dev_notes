```
#!/usr/bin/env bash -ue  
  
function cmd_help {  
  cat << 'HEREDOC'  
  
Usage:  
  tool [options] < command > [ < command args > ]  
  
Options:  
  --help                                show this help message  

Commands:  
  
HEREDOC  
  exit 1  
}  
  
# ---  
# globals  
# ---  
  
FILENAME=$(basename "$0")  
COMMAND=""  
EXIT_CODE=0
DRY_RUN=0  
  
# ---  
# helper functions  
# ---  
  
function log {  
  if [[ "$QUIET" == 0 ]]; then  
    echo "[$FILENAME][$COMMAND][$COMMAND_STATUS]: $@"  
  fi  
}  
  
function log_error {  
  >&2 echo "[$FILENAME][ERROR]: $@"  
}  
  
function ensure_command {  
  if ! command -v $1 &> /dev/null; then  
    log_error "'$1' command could not be found (suggest: $2)"  
    exit 1  
  fi  
}  
  
function escape_double_quotes {  
  sed -e 's|"|\\"|g' <<< "$1"  
}  
  
# if a string contains spaces surround it with double quotes and escape any double quotes in the string  
function maybe_quote_string {  
  local STR=$1  
  
  IS_DOUBLE_QUOTED=0  
  if [[ "$STR" == "\""* && "$STR" == *"\"" ]]; then  
    IS_DOUBLE_QUOTED=1  
  fi  
  
  IS_SINGLE_QUOTED=0  
  if [[ "$STR" == "'"* && "$STR" == *"'" ]]; then  
    IS_SINGLE_QUOTED=1  
  fi  
  
  if [[ IS_DOUBLE_QUOTED == 0 && IS_SINGLE_QUOTED == 0 && "$STR" == *" "* ]]; then  
     echo "\"$(escape_double_quotes "$STR")\""  
   else  
     echo "$STR"  
  fi  
}  
  
function cmd_array_to_cmd_string {  
  local CMD_ARR=("$@")  
  
  local CMD_STRING="${CMD_ARR[0]}"  
  for arg in "${CMD_ARR[@]:1}"; do  
    # double quote arguments containing spaces and escape double quotes  
    MAYBE_QUOTED_STRING=$(maybe_quote_string "$arg")  
    CMD_STRING="$CMD_STRING $MAYBE_QUOTED_STRING"  
  done  
  
  echo "$CMD_STRING"  
}  
  
function run_bash_cmd {  
  local CMD=$1  
  local CMD_OPTIONS=$2  
  shift  
  shift  
  local CMD_ARGS=("$@")  
  local CMD_STRING="$CMD${CMD_OPTIONS} $(cmd_array_to_cmd_string "${CMD_ARGS[@]}")"  
  
  if [[ "$DRY_RUN" == 0 ]]; then  
    bash -c "$CMD_STRING"  
  else  
    echo "$CMD_STRING"  
  fi  
}  
  
# ---  
# command handlers  
# ---  
  
function cmd_example {  

}  
    
# ---  
# validate  
# ---  
  
(  
  ensure_command "some command" "some suggestion" &  
  ensure_command "other command" "another suggestion"
) || exit $?  
  
# ---  
# parse command line  
# ---  
  
while [[ -z "$COMMAND" ]]; do  
  
  if [[ $# == 0 ]]; then  
    log_error "missing command"  
    cmd_help  
  fi  
  
  case $1 in  
    --help)  
      cmd_help  
      ;;  
    --dry-run)  
      DRY_RUN=1  
      shift # past argument  
      ;;  
    -q|--quiet)  
      QUIET=1  
      shift # past argument  
      ;;  
    --*|-*)  
      log_error "unknown option: $1"  
      cmd_help  
      ;;  
    *)  
   	  COMMAND="$1"  
      shift  
      ;;  
 . esac
done  
  
  
# ---  
# dispatch command  
# ---  
  
case $COMMAND in  
  example)  
    ;;  

  *)  
    log_error "unknown command: $COMMAND"  
    cmd_help  
    ;;  
esac

exit $EXIT_CODE
```