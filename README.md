# Load-Analysis-Using-dcpumonview
***
To analyze the resource consumption on your cPanel server, you can use the `dcpumonview` command which provides detailed information about CPU, memory, and MySQL usage by each account. Here is how you can execute it:

### Code
``` js
domain="example.com"; for i in $(seq 1 7); do let i=$i+1; let k=$i-1; let s="$(date +%s) - ($(($k-1))*86400)"; let t="$(date +%s) - ($(($k-2))*86400)"; echo $(date -I date -d "@$s"); /usr/local/cpanel/bin/dcpumonview $(date -d "@$s +%s") $(date -d "@$t +%s") | sed -r -e 's@^<tr bgcolor=#[[:xdigit:]]+><td>(.*)</td><td>(.*)</td><td>(.*)</td><td>(.*)</td><td>(.*)</td></tr>$@Account: \1\tDomain: \2\tCPU: \3\tMem: \4\tMySQL: \5@' -e 's@^<tr><td>Top Process</td><td>(.*)</td><td colspan=3>(.*)</td></tr>$@\1 - \2@' | grep $domain -A3; done
```
This will provide you with details about the CPU, memory, and MySQL usage for the specified domain over the last seven days. It will display the top processes used during that period, helping you identify what might be causing the increased resource usage.
```
USER       CPU
user1        2.51%
user2        1.48%
user3        1.04%
user4        0.77%
user5        0.61%

USER       MEMORY
user1        5.67%
user2        0.38%
user3        0.38%
user4        0.13%
user5        0.12%

USER       MYSQL
user1        0.3%
user2        0.0%
user3        0.0%
user4        0.0%
user5        0.0%
```

### Analyzing High Resource Domains and User Stats
***
After identifying high resource-consuming domains using the initial analysis, you can delve deeper into user statistics for the past 5 days. Execute the following command in SSH to gather detailed insights:
### code
``` js
domain="DOMAIN.com"; for i in `seq 1 7 `; do let i=$i+1 ; let  k=$i-1 ; let s="$(date +%s) - (k-1)*86400"; let t="$(date +%s) - (k-2)*86400"; echo `date -Idate -d @$s`; /usr/local/cpanel/bin/dcpumonview `date -d @$s +%s` `date -d @$t +%s` | sed -r -e 's@^<tr bgcolor=#[[:xdigit:]]+><td>(.*)</td><td>(.*)</td><td>(.*)</td><td>(.*)</td><td>(.*)</td></tr>$@Account: \1\tDomain: \2\tCPU: \3\tMem: \4\tMySQL: \5@' -e 's@^<tr><td>Top Process</td><td>(.*)</td><td colspan=3>(.*)</td></tr>$@\1 - \2@' | grep $domain -A3 ; done
```

### command breakdown
***
- **domain="DOMAIN.com"**: Replace "DOMAIN.com" with the specific domain you want to analyze.
- **for i in $(seq 1 7)**: Iterates over the last 7 days.
- **$(date -Idate -d "@$s")**: Displays the date for each iteration.
- **/usr/local/cpanel/bin/dcpumonview**: Retrieves CPU, memory, and MySQL usage data.
- **sed ...**: Formats the output to display account, domain, CPU, memory, and MySQL usage.
- **grep $domain -A3**: Filters results for the specified domain and shows top processes.
- 
This command provides a comprehensive overview of user statistics for the chosen domain over the past 5 days, aiding in pinpointing resource-intensive processes and optimizing server performance.

### Links
***
(https://www.hungred.com/how-to/awesome-cpanel-commands/)
