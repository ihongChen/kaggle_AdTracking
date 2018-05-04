# Note for features testing

1. [x] rolling on ip --> ip fail nohelp (4,5)
2. [x] channel_cumcnt_ip_dev_os (6) 
3. [x] channel_cumcnt_ip_dev_os_app_day_hour(7_1)
4. [x] # of unique 'app' by ('ip','device','os') (8) --X3_1
5. [x] # of unique 'app' by ('ip','device','os','channel') (9) --X3_2
6. [x] nextclick timedelta by ('ip','device','os','app','channel')
7. [o] ip -> not category, but integer (imporved!!)
8. [ ] ip,app,os,channel 
9. [ ] shift1_iptcnt

mean encode:
1. [x] by (ip,app,os,device) --- (useless)
2. [x] by (ip,app) ---(useless)
3. [x] by (app,channel) ---(useless)

