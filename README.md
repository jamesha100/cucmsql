# CUCM SQL Commands

This Github Pages site was created to document useful SQL commands for Cisco Unified Communications Manager.  
Most of the testing was performed on CUCM versions 11.5 and 12.5.

---
## Extension Mobility Commands
### List Currently Logged In Users and Devices
The SQL command below lists the following information:
- Device Pool
- Device Type
- User ID
- Device Name
- Login date and time (UTC)

```
run sql SELECT dp.name AS devicepool, tm.name AS devicetype, eu.userid AS userid, d.name AS devicename, dbinfo('utc_to_datetime', emd.datetimestamp) AS logintime FROM extensionmobilitydynamic emd INNER JOIN enduser eu on emd.fkenduser=eu.pkid INNER JOIN device d on emd.fkdevice=d.pkid INNER JOIN typemodel tm on d.tkmodel=tm.enum INNER JOIN devicepool dp on d.fkdevicepool=dp.pkid ORDER BY dp.name,tm.name,eu.userid,d.name

devicepool     devicetype            userid               devicename      logintime
============== ===================== ==================== =============== ===================
Charlton_House Cisco 7841            Emma.Holtom          SEP08CCA7F6C685 2019-07-11 11:06:41
Charlton_House Cisco 7911            Joe.Hughes           SEP9CAFCAFF6059 2019-07-01 08:18:09
Charlton_House Cisco 7911            Luke.Mann            SEP04C5A44D08CC 2018-11-09 03:14:59
Charlton_House Cisco 7911            david.white          SEP9CAFCAFEC763 2018-11-02 11:52:07
Charlton_House Cisco 7911            duncan.richards      SEP0021A0D88A50 2016-11-23 16:21:25
Charlton_House Cisco 7911            michael.agolom       SEP9CAFCAFEC7F4 2017-07-19 10:51:39
```
### List Extension Mobility Profiles that are not Associated with an End User
The SQL command below list Extension Mobility profiles that are not associated with an End User.  
These profiles cannot be used and are likely to belong to users who have left the organisation.

```
run sql SELECT name FROM device d WHERE NOT EXISTS (SELECT fkdevice FROM enduserdevicemap eudm WHERE d.pkid = eudm.fkdevice) AND tkclass = '254' order by name

name
==================================================
AS ADMIN - 5686
Adrian Allanach - 2160
Alice Green - 2389
Amanda Janulis - 2189
Amir Amini - 5879
Andrew Boulter - 5512

```
