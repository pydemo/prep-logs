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
pattern ${VAR#\*/}|delete shortest part that matches from the beginnning 
pattern ${VAR##\*/}| delete longest part that matches from the beginnning 
pattern ${VAR%/\*}|delete shortest part that matches from the end
pattern ${VAR%%\/\*}| delete longest part that matches from the end
External calculation| let x="$1 $2 $3"
bash internal calculator| echo $(( 10\*30 ))
sorting by field name|sort  -k1 -t : /etc/passwd
sorting field 1|cut -f 1 -d:/etc/passwd
show  line \#5| sed -n 5p /etc/passwd
substitute in file|sed -i s/hello/bye/g file.txt
delete line \#5|sed -i -e '5d' file.txt
show column for pattern|awf -F : '/alex/ {print $4}] /etc/passwd
