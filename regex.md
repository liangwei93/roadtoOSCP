grep -R 4.0.5 . | awk -F: '{print $1}' |uniq                                                                                                                 
./admin/boot/rules/98-constants.bit
./admin/js/tinymce/skins/lightgray/fonts/tinymce-small.svg
./admin/js/tinymce/skins/lightgray/fonts/tinymce.svg


ind . | grep controller                                                                          
./admin/controllers
./admin/controllers/dashboard
./admin/controllers/dashboard/view.bit
./admin/controllers/post



http://10.10.10.75/nibbleblog/content/private/users.xml

<users>
<user username="admin">
<id type="integer">0</id>
<session_fail_count type="integer">1</session_fail_count>
<session_date type="integer">1623766781</session_date>
</user>
<blacklist type="string" ip="10.10.10.1">
<date type="integer">1512964659</date>
<fail_count type="integer">1</fail_count>
</blacklist>
<blacklist type="string" ip="10.10.16.4">
<date type="integer">1623766781</date>
<fail_count type="integer">1</fail_count>
</blacklist>
</users>