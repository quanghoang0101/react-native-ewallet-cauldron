# Workflow to Mobile Native app (iOS, Android) consume Electrode Native Container

**1. Regen Container**

`ern cauldron regen-container -v [containerVersion] --resetCache true --fullRegen`

**2. Transform Container**
   
   IOS: `ern transform-container -t xcframework -p ios`

**3. Public Container**

   IOS: `ern publish-container -p cocoapod-git -u [url_to_git_repo] -v [gitTag]  --platform ios -e '{"branch":"[branch_name]"}'`
   Android: `ern publish-container -p git -u [url_to_git_repo] -v [gitTag]  --platform android -e '{"branch":"[branch_name]"}'`

**3. Consume Container**

  IOS: `pod 'ElectrodeContainer', :git => 'url_to_git_repo', :tag => 'container_version'`
  
  Android: update build.gradle.kt
  
      android {
         repositories {
           maven { url = URI("https://jitpack.io") }
         }
      }

      dependencies {
         implementation("com.github.[username]:[git-repo-name]:[tag]")
      }
      
      Example:
       dependencies {
         implementation("com.github.quanghoang0101:ewallet-container-android:1.0.2")
      }
