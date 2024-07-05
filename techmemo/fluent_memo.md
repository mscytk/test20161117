```
root@stgesslog001:~# cat /etc/td-agent/td-agent.conf
####
## Source descriptions:
##

<source>
  @type forward
  port 24224
</source>


####
## Filter descriptions:
##

## Filters go here.


####
## Match descriptions:
##

<match app.repica.*>
  @type copy

  <store>
    @type mysql
    host 127.0.0.1
    port 3307
    database ess
    username essadmin
    password spleen
    key_names niftyid,utc_ts,status,status_code,transaction_id,envfrom,toaddr,connect_to,connect_from,timing_connection_closed,messages,logfilename,status,utc_ts
    sql INSERT into repicalog (NIFTYID, LOGTIMESTAMP, STATUS, STATUSCODE, TRANSACTIONID, FROMADDRESS, TOADDRESS, TOIPPORT, SRCIPPORT, SMTPTIMING, MESSAGE, LOGFILENAME) VALUES(?,?,?,?,?,?,?,?,?,?,?,?) on duplicate key update STATUS=?, LOGTIMESTAMP=?;
    <buffer>
      @type file
      path /var/log/td-agent/buffer/mysql_repica.log
      flush_interval 30s
    </buffer>
  </store>

  <store>
    @type record_reformer
    tag app.repica.tofile.${tag_parts[2]}
  </store>

  <store>
    @type record_reformer
    tag app.repica.tostorage_v2.${tag_parts[2]}
  </store>
</match>

<match app.repica.tofile.*>
  @type forest
  subtype file
  <template>
    path /var/log/repica_log_arch/${tag_parts[3]}
    append true
    buffer_path /var/log/td-agent/buffer/repica.${tag_parts[3]}
    output_tag false
    output_time false
    time_slice_format %Y%m%d%H%M
    time_slice_wait 3m
    compress gzip
    retry_wait 5
    retry_limit 3
    <format>
      @type single_value
      message_key line_raw
      add_newline true
    </format>
  </template>
</match>

<match app.repica.tostorage_v2.*>
  @type forest
  subtype file
  <template>
    path /var/log/repica_log_arch_cls_v2/buffer/${tag_parts[3]}
    append true
    buffer_path /var/log/td-agent/buffer/repica_cld_v2.${tag_parts[3]}
    output_tag false
    output_time false
    time_slice_format %Y%m%d%H
    time_slice_wait 20m
    retry_wait 5
    retry_limit 3
    <format>
      @type single_value
      message_key line_raw
      add_newline true
    </format>
  </template>
</match>
```
各dir
```
root@stgesslog001:~# ls /var/log/repica_log_arch
BCT27528.202407031917.log.gz  BCT29902.202407041853.log.gz  BCT44928.202407031917.log.gz  BCT53434.202407031917.log.gz  BCT59963.202407031917.log.gz  BCT66717.202407031917.log.gz  FJC26878.202407031917.log.gz
BCT27565.202407031917.log.gz  BCT29902.202407041908.log.gz  BCT45375.202407031917.log.gz  BCT54048.202407031917.log.gz  BCT60233.202407031917.log.gz  BCT66994.202407031917.log.gz  FJC26910.202407031917.log.gz
BCT27688.202407031917.log.gz  BCT29902.202407041923.log.gz  BCT45620.202407031917.log.gz  BCT54714.202407031917.log.gz  BCT61095.202407031917.log.gz  ESSADMIN.202407031917.log.gz  FJC26934.202407031917.log.gz
BCT28795.202407031917.log.gz  BCT29902.202407041938.log.gz  BCT46166.202407031917.log.gz  BCT55027.202407031917.log.gz  BCT61484.202407031917.log.gz  FJC20157.202407031917.log.gz  FJC27039.202407031917.log.gz
BCT29716.202407031917.log.gz  BCT29902.202407041953.log.gz  BCT46172.202407031917.log.gz  BCT55581.202407031917.log.gz  BCT63066.202407031917.log.gz  FJC20274.202407031917.log.gz  FJC27253.202407031917.log.gz
BCT29901.202407031917.log.gz  BCT29902.202407042008.log.gz  BCT46689.202407031917.log.gz  BCT55643.202407031917.log.gz  BCT64411.202407031917.log.gz  FJC20401.202407031917.log.gz  FJC29475.202407031917.log.gz
BCT29902.202407031917.log.gz  BCT30491.202407031917.log.gz  BCT47137.202407031917.log.gz  BCT55849.202407031917.log.gz  BCT64465.202407031917.log.gz  FJC20611.202407031917.log.gz  FJC29506.202407031917.log.gz
BCT29902.202407041616.log.gz  BCT34432.202407031917.log.gz  BCT47152.202407031917.log.gz  BCT56144.202407031917.log.gz  BCT64642.202407031917.log.gz  FJC22360.202407031917.log.gz  FJC29566.202407031917.log.gz
BCT29902.202407041624.log.gz  BCT38139.202407031917.log.gz  BCT48228.202407031917.log.gz  BCT56276.202407031917.log.gz  BCT64643.202407031917.log.gz  FJC22416.202407031917.log.gz  FJC29842.202407031917.log.gz
BCT29902.202407041653.log.gz  BCT38141.202407031917.log.gz  BCT48237.202407031917.log.gz  BCT56880.202407031917.log.gz  BCT65059.202407031917.log.gz  FJC22578.202407031917.log.gz  FJC30096.202407031917.log.gz
BCT29902.202407041658.log.gz  BCT38415.202407031917.log.gz  BCT48729.202407031917.log.gz  BCT57356.202407031917.log.gz  BCT65337.202407031917.log.gz  FJC22787.202407031917.log.gz  FJC30832.202407031917.log.gz
BCT29902.202407041703.log.gz  BCT41280.202407031917.log.gz  BCT48801.202407031917.log.gz  BCT57797.202407031917.log.gz  BCT65585.202407031917.log.gz  FJC23625.202407031917.log.gz  FJC30972.202407031917.log.gz
BCT29902.202407041708.log.gz  BCT41302.202407031917.log.gz  BCT49981.202407031917.log.gz  BCT58831.202407031917.log.gz  BCT66069.202407031917.log.gz  FJC24057.202407031917.log.gz  FJC31223.202407031917.log.gz
BCT29902.202407041738.log.gz  BCT41326.202407031917.log.gz  BCT50646.202407031917.log.gz  BCT58845.202407031917.log.gz  BCT66119.202407031917.log.gz  FJC24991.202407031917.log.gz  FJC32838.202407031917.log.gz
BCT29902.202407041753.log.gz  BCT42579.202407031917.log.gz  BCT51010.202407031917.log.gz  BCT58879.202407031917.log.gz  BCT66265.202407031917.log.gz  FJC25743.202407031917.log.gz  FJC33760.202407031917.log.gz
BCT29902.202407041808.log.gz  BCT43482.202407031917.log.gz  BCT51561.202407031917.log.gz  BCT58914.202407031917.log.gz  BCT66279.202407031917.log.gz  FJC25777.202407031917.log.gz  FJC34512.202407031917.log.gz
BCT29902.202407041823.log.gz  BCT44544.202407031917.log.gz  BCT52895.202407031917.log.gz  BCT59004.202407031917.log.gz  BCT66300.202407031917.log.gz  FJC25803.202407031917.log.gz  ZYP09685.202407031917.log.gz
BCT29902.202407041838.log.gz  BCT44805.202407031917.log.gz  BCT53267.202407031917.log.gz  BCT59754.202407031917.log.gz  BCT66641.202407031917.log.gz  FJC26871.202407031917.log.gz
root@stgesslog001:~# ls /var/log/repica_log_arch
repica_log_arch/        repica_log_arch_cls_v2/ 
root@stgesslog001:~# ls /var/log/repica_log_arch_cls_v2/
archv  buffer
root@stgesslog001:~# ls /var/log/repica_log_arch_cls_v2/buffer/
ESSADMIN.2024070319.log
root@stgesslog001:~# ls /var/log/repica_log_arch_cls_v2/archv/
BCT29902.20240704.zip
```
