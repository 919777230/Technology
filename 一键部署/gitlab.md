## gitlab

```base
 docker pull gitlab/gitlab-ce
docker volume create gitlab
docker run --name gitlab --restart=always -p 9980:9980 -p 222:22 -v /var/lib/docker/volumes/gitlab/config:/etc/gitlab -v /var/lib/docker/volumes/gitlab/logs:/var/log/gitlab -v /var/lib/docker/volumes/gitlab/data:/var/opt/gitlab -d gitlab/gitlab-ce

# 修改配置
docker exec -it gitlab bash

vi /etc/gitlab/gitlab.rb 
## GitLab URL
##! URL on which GitLab will be reachable.
##! For more details on configuring external_url see:
##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab
##!
##! Note: During installation/upgrades, the value of the environment variable
##! EXTERNAL_URL will be used to populate/replace this value.
##! On AWS EC2 instances, we also attempt to fetch the public hostname/IP
##! address from AWS. For more details, see:
##! https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html
# external_url 'GENERATED_EXTERNAL_URL'
external_url 'http://192.168.1.10:8800'
gitlab_rails['gitlab_ssh_host'] = '192.168.1.10'
gitlab_rails['gitlab_shell_ssh_port'] = 222

# 重新编译
gitlab-ctl reconfigure
# 重启
gitlab-ctl restart
```
