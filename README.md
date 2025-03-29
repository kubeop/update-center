# jenkins-update-center  

[![](https://data.jsdelivr.com/v1/package/gh/kubeop/update-center/badge)](https://www.jsdelivr.com/package/gh/kubeop/update-center)
![workflow build](https://github.com/kubeop/update-center/actions/workflows/mirrors.yml/badge.svg)
![star](https://img.shields.io/github/stars/kubeop/update-center?color=green&style=social)

本脚本由[lework/jenkins-update-center](https://github.com/lework/jenkins-update-center)修改而来，仅支持LTS版本Jenkins配置。



## mirror site speed test

```shell
curl -sSL https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/main/speed-test.sh | bash
```



## Generate self-signed certificate

```shell
openssl genrsa -out rootCA/update-center.key 2048
openssl req -new -x509 -days 3650 -key rootCA/update-center.key -out rootCA/update-center.crt
```



## update-center.json

| Site     | Source                                                       | Proxy                                                        |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| aliyun   | https://raw.githubusercontent.com/kubeop/update-center/master/updates/aliyun/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/aliyun/update-center.json |
| bfsu     | https://raw.githubusercontent.com/kubeop/update-center/master/updates/bfsu/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/bfsu/update-center.json |
| huawei   | https://raw.githubusercontent.com/kubeop/update-center/master/updates/huawei/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/huawei/update-center.json |
| nju      | https://raw.githubusercontent.com/kubeop/update-center/master/updates/nju/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/nju/update-center.json |
| tencent  | https://raw.githubusercontent.com/kubeop/update-center/master/updates/tencent/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/tencent/update-center.json |
| tsinghua | https://raw.githubusercontent.com/kubeop/update-center/master/updates/tsinghua/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/tsinghua/update-center.json |
| ustc     | https://raw.githubusercontent.com/kubeop/update-center/master/updates/ustc/update-center.json | https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/ustc/update-center.json |



## Usage mirror site

1. Upload custom CA file.

    ```shell
    [ ! -d /var/lib/jenkins/update-center-rootCAs ] && mkdir /var/lib/jenkins/update-center-rootCAs
    wget https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/refs/heads/main/rootCA/update-center.crt -O /var/lib/jenkins/update-center-rootCAs/update-center.crt
    chown jenkins.jenkins -R /var/lib/jenkins/update-center-rootCAs
    ```

    
    
2. Change Update Site url.

   ```shell
   sed -i 's#https://updates.jenkins.io/update-center.json#https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/ustc/update-center.json#' /var/lib/jenkins/hudson.model.UpdateCenter.xml
   rm -f /var/lib/jenkins/updates/default.json
   
   systemctl restart jenkins
   ```
   
   > Or it can be modified on the web.
   >
   > Go to `Jenkins` → `Manage Jenkins` → `Manage Plugins` → `Advanced` → Update Site and submit URL to your `https://gh-proxy.com/https://raw.githubusercontent.com/kubeop/update-center/master/updates/ustc/update-center.json`

