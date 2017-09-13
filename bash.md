# Bash prep-logs
Q | Info 
--- | ---
debug|bash +x test.sh
shift|Removes first arg $1
wall|cat, heredoc
substitution show|${VAR:-word}
substitution set and show|${VAR:=word}
substitution append|${VAR:?word}
substitution substring|${VAR:offset:length}
pattern ${VAR#\*/}|delete shortest part that matches
pattern ${VAR##\*/}| delete longest part that matches
