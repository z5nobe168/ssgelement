# 设置平台版本 (Element 1.11.25 要求 iOS 13.0+)
platform :ios, '13.0'

# 禁止生成多项目结构 (根据项目需要)
install! 'cocoapods', generate_multiple_pod_projects: false

# 主应用目标
target 'Element' do
  use_frameworks! :linkage => :static  # 推荐使用静态框架以减少包体积
  
  # Flutter 集成
  flutter_application_path = '../'
  load File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')
  install_all_flutter_pods(flutter_application_path)
  
  # Element 核心依赖
  pod 'MatrixSDK', '~> 0.24.8'        # 1.11.25 使用的 Matrix SDK 版本
  pod 'Realm', '~> 10.42.0'           # 数据库依赖
  pod 'SwiftyBeaver', '~> 1.9.6'       # 日志系统
  
  # UI 组件
  pod 'SDWebImage', '~> 5.15.0'
  pod 'SnapKit', '~> 5.6.0'
  
  # 通知扩展目标
  target 'OneSignalNotificationServiceExtension' do
    pod 'MatrixSDK', '~> 0.24.8'
  end

  # 所有目标共享的设置
  post_install do |installer|
    installer.pods_project.targets.each do |target|
      target.build_configurations.each do |config|
        # 设置 Swift 版本
        config.build_settings['SWIFT_VERSION'] = '5.0'
        
        # 禁用 Bitcode
        config.build_settings['ENABLE_BITCODE'] = 'NO'
        
        # 解决 Xcode 14 兼容问题
        config.build_settings['EXCLUDED_ARCHS[sdk=iphonesimulator*]'] = 'arm64'
      end
    end
    
    # Flutter 后安装脚本
    flutter_post_install(installer) if defined?(flutter_post_install)
  end
end
