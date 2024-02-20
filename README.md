# Workflow to Mobile Native app (iOS, Android) consume Electrode Native Container

**1. Generate Container**

`ern cauldron regen-container -v [containerVersion] --resetCache true --fullRegen`

**2. Transform Container**
   
   IOS: If you are using on MAC with chip Apple and Xcode 15. You should update the Podfile on Container:
   
      post_install do |installer|
          react_native_post_install(installer)
          __apply_Xcode_12_5_M1_post_install_workaround(installer)
          __xcode_15_workaround(installer)
          
          ... #the rest of code ...
          
      end
   
      def __xcode_15_workaround(installer)
        xcode_version_output = `xcodebuild -version`
        xcode_version_match = xcode_version_output.match(/Xcode (\d+(\.\d+)?)/)
      
        if xcode_version_match
          xcode_version = Gem::Version.new(xcode_version_match[1])
          if xcode_version >= Gem::Version.new('15.0')
            installer.pods_project.targets.each do |target|
              target.build_configurations.each do |config|
                config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= ['$(inherited)', '_LIBCPP_ENABLE_CXX17_REMOVED_UNARY_BINARY_FUNCTION']
              end
            end
          end
        end
      end
      
Then tranform: 
      `ern transform-container -t xcframework -p ios`
      

**3. Public Container**

   **IOS**:
   
   `ern publish-container -p cocoapod-git -u [url_to_git_repo] -v [gitTag]  --platform ios -e '{"branch":"[branch_name]"}'`
   
   **Android**: 
   
   `ern publish-container -p git -u [url_to_git_repo] -v [gitTag]  --platform android -e '{"branch":"[branch_name]"}'`

**3. Consume Container**

  **IOS**: 
  `pod 'ElectrodeContainer', :git => 'url_to_git_repo', :tag => 'container_version'`
  
  **Android**: update build.gradle.kt
     
      android {
         repositories {
           maven { url = URI("https://jitpack.io") }
         }
      }

      dependencies {
         implementation("com.github.[username]:[git-repo-name]:[tag]")
      }
      
   Example:
   
   ```
dependencies {
         implementation("com.github.quanghoang0101:ewallet-container-android:1.0.2")   
   }  
