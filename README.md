# Steps to Public Container

**1. Regen Container**

`ern cauldron regen-container -v [containerVersion] --resetCache true --fullRegen`

**2. Transform Container**
   
   IOS: `ern transform-container -t xcframework -p ios`

**3. Public Container**

   IOS: `ern publish-container -p cocoapod-git -u [url_to_git_repo] -v [gitTag]  --platform ios -e '{"branch":"[branch_name]"}'`
   Android: `ern publish-container -p git -u [url_to_git_repo] -v [gitTag]  --platform android -e '{"branch":"[branch_name]"}'`

**3. Consume Container**

  IOS: `pod 'ElectrodeContainer', :git => 'url_to_git_repo', :tag => 'container_version'`
