# jenkins-update-center  

![GitHub](https://img.shields.io/github/license/lework/jenkins-update-center)
[![](https://data.jsdelivr.com/v1/package/gh/lework/jenkins-update-center/badge)](https://www.jsdelivr.com/package/gh/lework/jenkins-update-center)

本脚本由[lework/jenkins-update-center](https://github.com/lework/jenkins-update-center)修改而来，仅支持LTS版本Jenkins配置。



## mirror site speed test

```
curl -sSL https://cdn.jsdelivr.net/gh/k8sre/update-center/speed-test.sh | bash
```



## update-center.json

| Site   | Source                                                       | CDN                                                          |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| huawei | https://raw.githubusercontent.com/k8sre/update-center/master/updates/huawei/update-center.json | https://cdn.jsdelivr.net/gh/k8sre/update-center/updates/huawei/update-center.json |
| ustc   | https://raw.githubusercontent.com/k8sre/update-center/master/updates/ustc/update-center.json | https://cdn.jsdelivr.net/gh/k8sre/update-center/updates/ustc/update-center.json |



## Usage mirror site

1. Upload custom CA file.

    ```bash
    [ ! -d /var/lib/jenkins/update-center-rootCAs ] && mkdir /var/lib/jenkins/update-center-rootCAs
    wget https://cdn.jsdelivr.net/gh/k8sre/update-center/rootCA/update-center.crt -O /var/lib/jenkins/update-center-rootCAs/update-center.crt
    chown jenkins.jenkins -R /var/lib/jenkins/update-center-rootCAs
    ```

    
    
2. Change Update Site url.

   ```bash
   sed -i 's#https://updates.jenkins.io/update-center.json#https://cdn.jsdelivr.net/gh/k8sre/update-center/updates/ustc/update-center.json#' /var/lib/jenkins/hudson.model.UpdateCenter.xml
   rm -f /var/lib/jenkins/updates/default.json
   
   systemctl restart jenkins
   ```
   
   > Or it can be modified on the web.
   >
   > Go to `Jenkins` → `Manage Jenkins` → `Manage Plugins` → `Advanced` → Update Site and submit URL to your `https://cdn.jsdelivr.net/gh/k8sre/update-center/updates/ustc/update-center.json`


