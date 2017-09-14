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
$NF| last field of awk
awk filter |'$3>500'
awk filter| awk -F : '$NF ~/bash/' /etc/passwd
uppercase string| tr [a-z][A-Z] or tr [:lower:] [:upper:]
for loop|for i in \`cat users\`; do echo $i; done
for loop int |for i in {200..210}; do ping -c 1 192.168.234.$1 2>/dev/null; done or for i in {200..210}; do ping -c 1 192.168.234.$1 2>/dev/null; done
Incrementing| COUNTER=$((COUNTER+1))
file count| for i in \*
until user logged in |```bash
until users|grep $1 > /dev/null
do
  echo $1 is not logged in yet
  sleep 5
done

echo $1 has just logged in
```
