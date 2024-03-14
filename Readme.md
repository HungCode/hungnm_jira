1. Copy atlassian-extras-3.2.jar to folder  atlassian-jira-software-8.3.2-standalone/atlassian-jira/lib  
and  copy atlassian-universal-plugin-manager-plugin-4.0.1.jar to folder atlassian-jira-software-8.3.2-standalone/atlassian-jira/atlassian-bundled-plugins/ 
2. Config home folder for jira: vi classes/jira-application.properties
3. Thay đổi config database trong file seting xml nếu cần
jira.home = /opt/atlassian/jira/
4. Start jira server     atlassian-jira-software-8.3.2-standalone/bin/start-jira.sh 
5. Copy Server ID vao file: license_key.txt
6.Run command:  php atlassian-keygen.php -e license_key.txt 
7. Copy key generate in console
