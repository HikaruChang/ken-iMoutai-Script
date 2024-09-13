# ken-iMoutai-Script

`ken-iMoutai-Script` 是一个青龙脚本，旨在自动化完成 i茅台的预约申购、登录、短信验证码处理、耐力值和小茅运领取以及旅行功能。



## 功能

- **登录**

- **短信验证码**

- **预约申购**

- **耐力值和小茅运领取**

- **旅行功能**



## 使用方法

0. **拉取仓库：**

    在青龙面板-订阅管理-创建订阅，复制以下命令到`名称`输入框即可自动配置订阅链接，定时规则自定，例如`0 5 * * *`，添加完成后点击列表行`运行`，脚本会自动添加到`定时任务`。

    ```
    ql repo https://github.com/AkenClub/ken-iMoutai-Script.git
    ```

1. **获取登录验证码**

   运行 `1_generate_code.py` 脚本以获取登录验证码，并记录设备ID、版本号等数据。

   - 变量配置：

   ```
   # 填写手机号码，收到验证码后在 2_login.py 中填写验证码
   PHONE_NUMBER = ''
   ```

2. **登录**

   使用获取到的验证码、设备ID 和版本号进行登录请求，输出用户信息并记录以备后续使用。请运行 `2_login.py` 脚本。

   - 变量配置：


   ```
   # 手机号码，和上一步的手机号码一致
   PHONE_NUMBER = ''
   # 验证码，填写收到的验证码
   CODE = ""
   # 设备 ID，和上一步的设备 ID 一致
   DEVICE_ID = ""
   # 版本号，和上一步的版本号一致
   MT_VERSION = ""
   ```

3. 查询可预约商品和售卖商店

   运行 `3_retrieve_shop_and_product_info.py` 脚本查询可预约的商品和商店信息，获取 SHOP_ID、LAT、LNG 等数据。

   - 变量配置：

   ```
   # 省份
   PROVINCE_NAME = "广西壮族自治区"
   # 城市
   CITY_NAME = "南宁市"
   ```

4. 预约商品

   每日 9:10 开始预约，并根据青龙面板的通知渠道进行通知。请运行 `4_reserve_product.py` 脚本。

   - **配置环境变量 `KEN_IMAOTAI_ENV`：**

     内容格式为：    PHONE_NUMBER$USER_ID$DEVICE_ID$MT_VERSION$PRODUCT_ID_LIST$SHOP_ID$LAT$LN G$TOKEN$COOKIE

     **解释**：手机号码$用户ID$设备ID$版本号$商品ID列表$店铺ID$纬度$经度$TOKEN$COOKIE
     **多个用户时使用 & 连接**。

     常量。
     - PHONE_NUMBER: 用户的手机号码。            --- 自己手机号码
     - CODE: 短信验证码。                                         --- 运行 1_generate_code.py 获取
     - DEVICE_ID: 设备的唯一标识符。                     --- 运行 1_generate_code.py 获取
     - MT_VERSION: 应用程序的版本号。                 --- 运行 1_generate_code.py 获取
     - USER_ID: 用户的唯一标识符。                          --- 运行 2_login.py 获取
     - TOKEN: 用于身份验证的令牌。                          --- 运行 2_login.py 获取
     - COOKIE: 用于会话管理的Cookie。                     --- 运行 2_login.py 获取
     - PRODUCT_ID_LIST: 商品ID列表，表示用户想要预约的商品。--- 运行 3_retrieve_shop_and_product_info.py 获取
     - SHOP_ID: 店铺的唯一标识符。                     --- 运行 3_retrieve_shop_and_product_info.py 获取
     - LAT: 用户所在位置的纬度。                           --- 运行 3_retrieve_shop_and_product_info.py 获取
     - LNG: 用户所在位置的经度。                          --- 运行 3_retrieve_shop_and_product_info.py 获取

     **注意**：PRODUCT_ID_LIST 里面的 ID 值需要**用单引号**。

     例如：

     ```
     IMAOTAI_ENV = "13312345678$1000009$94b6b875-0e2b-447c-00d2-f0b1899be858$1.7.2$['10941', '10923', '2478', '10942']$244040684001$24.016768$114.135726$eyJ0...eXC6SRF9o$eyJYjZi...c1KxcYyk&13412345678$......
     ```

5. 旅行 & 获取小茅运、首次分享奖励

   每日从早上 9 点到晚上 8 点之间，每 7 个小时执行一次旅行任务。请运行 `5_travel_and_rewards.py` 脚本。9:15、16:15 执行，每日旅行两次应该足够用完耐力值了，可自行修改。

   - **配置环境变量 `KEN_IMAOTAI_ENV`：****同上**

6. 配置通知

   在脚本管理 - `AkenClub_ken-iMoutai-Script`的`notify.py`，配置 `push_config` 里面的字段，例如配置企业微信机器人，则填写 `QYWX_KEY` 的值即可。

   不需要通知时候内容末尾添加随机句子，可以配置 `HITOKOTO` 为 `False`。



## 计划

- ~~增加连续申购N天的小茅运检测和领取功能~~*`ad4101c`已完成，需重新拉库*
- ~~增加 7 日连续申购领取小茅运奖励功能~~`673dda4`已完成，需重新拉库



## 参考与致谢

本项目参考了以下两个项目：

- [yize8888/maotai](https://github.com/yize8888/maotai) - 提供了 i茅台自动化脚本的基础实现。
- [oddfar/campus-imaotai](https://github.com/oddfar/campus-imaotai) - 提供了类似的自动化功能和实现思路。

感谢这些项目的作者和贡献者，他们的工作为本项目提供了宝贵的参考和灵感。

>  `yize8888/maotai` 项目是青龙脚本，但没有登录、验证码获取、店铺信息、旅行相关等功能，使用时候需要额外抓包；
>
> `oddfar/campus-imaotai` 虽然功能齐全，但不是青龙脚本，对于原本已经运行青龙面板的用户来说，增加了额外的运行压力。
>
> 本项目将两者的优缺点融合在一起，主要用 python 重写了 oddfar/campus-imaotai 相关 Java 接口。



## 免责声明

本项目涉及抓取接口数据，仅用于学习和交流目的。请注意以下几点：

1. **合法性和合规性**： 使用本脚本时，请确保遵守所有相关法律法规以及服务条款。本项目的使用可能涉及到法律风险，用户需要对使用本脚本的行为负责。
2. **数据隐私**： 本脚本涉及对接口数据的抓取，用户需自行保证对其账号、数据和隐私的保护，避免泄露敏感信息。
3. **风险提示**： 由于抓取接口数据可能会受到系统限制或变更，脚本的正常运行和功能实现无法得到保证。使用本项目的风险由用户自行承担。
4. **第三方服务**： 本项目的部分功能可能依赖于第三方服务或接口，这些服务的变更或不可用可能会影响脚本的正常工作。
5. **学习和交流**： 本项目仅用于学习和交流目的，旨在帮助用户了解接口抓取和自动化处理的技术。请勿用于商业用途或其他非法活动。
6. **责任声明**： 本项目作者不对因使用本脚本而产生的任何直接或间接损失负责。请用户在使用前充分理解相关风险，并确保合法合规使用。



## 许可证

本项目使用 [Apache-2.0 许可证](LICENSE) 进行许可。有关更多详细信息，请参阅 `LICENSE` 文件。
