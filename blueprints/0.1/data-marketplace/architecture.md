# アプリケーションアーキテクチャー

データマーケットプレイスは、センサー、クラウドバックエンド、およびIOTAタングルを使用してデータをMAMチャンネルに公開します。

:::warning:免責事項
オープンソースプロジェクトを実行することは、不確実性とトレードオフを伴います。
以下に説明するアーキテクチャーが、同様のシステムを展開するのに役立つことを願っていますが、間違いが含まれている可能性があり、すべての状況に対処することはできません。
あなたのプロジェクトについて何か質問があれば、私たちはあなたがあなた自身の研究をし、専門家を探し、そしてIOTAコミュニティとそれらを話し合うことを奨励します。
:::

ここに記載されたPoCの構造（下の画像）は、センサーまたはその他のデバイス、クラウドバックエンド、および分散元帳（DLT）が相互接続されたパターンに従います。

![Data Marketplace architecture](../data-marketplace-architecture.png)

Submitting sensor data to the Data Marketplace is intended to be a lightweight operation that can be done by embedded devices. To submit sensor data, a device needs to perform Tangle operations, such as producing and consuming MAM channels, and communicating with web APIs. The data consuming part of the application is more complex and needs the ability to transfer IOTA tokens in exchange for access to the device's data streams. Therefore, an access-rights management add-on is implemented.

This table displays a list of all the main components of the application:

**Component**|**Description**
:-----|:------|
User authentication | OAuth with Google account, other types of authentication, like email/password can be enabled. Provides a unique API token and userID that are used for API communication and user identification.|
Database | NoSQL cloud database, provides a permanent storage mechanism. 
Cloud functions | Cloud functions (Lambda functions in AWS), that are triggered by an event or API call. Perform IOTA Tangle-related operations such as walled funding, token transfer for device stream purchase. Also perform Database-related operations such as creating used or device, managing device stream purchases. 
Hosting | Cloud hosting for the Web portal. 
Error logging | Provides access to error logs including request and response details to simplify issue investigations. 
Web portal | A web UI that allows easy device management and data stream fetching. 
API | Set of APIs to perform all main functionality from a script or console. 
Data submission template | Small NodeJS project with pre-filled device ID and API interface to Cloud backend. Provides examples for sensor data submission using different techniques. 

The cloud part of the application is centralized. It runs on Google Cloud Platform, and can optionally run on Amazon AWS or Microsoft Azure.
 
The centralized cloud backend consists of the following components:

- User authentication (OAuth with Google account)
- User management
- Access rights management
- Device management (create/read/delete)
- Wallet management (wallet funding, tokens transfer)
- Device stream purchase tracking
- Error tracking and reporting 

Wallet funding and token transfer operations are completed only after the node confirms that the transaction was attached to the Tangle.

Cloud functions can be configured to change the default values of the following:

* `depth` and `minWeightMagnitude` fields, which are typically different for IOTA Devnet and Mainnet nodes 
* Whitelist page, where administrator users with predefined email addresses are allowed to administer devices

## Sensors and data

The Data Marketplace is agnostic to the sensors and the data that you connect to it. Any sensor that has the ability to transmit data and has an easy way to get regular data readings, such as through an API, can be used with the Data Marketplace. In order to submit the data to the Marketplace, all you have to do is execute a script to submit data to the IOTA Tangle.
 
### Example sensors

* [Netatmo Weather Station](https://www.netatmo.com/en-us/weather)
* [Bosch XDK](https://xdk.bosch-connectivity.com/) 
* [Nordic Semiconductor Thingy:52](https://www.nordicsemi.com/Software-and-Tools/Development-Kits/Nordic-Thingy-52)
* [Raspberry Pi with a sensor kit](https://www.adafruit.com/product/2733) 

### Data fields

For each of your sensors, you have to define the correct data fields, which will be stored on the Tangle, and displayed on the Data Marketplace web portal for the purchaser. In general, just be descriptive with the data that you want to store and sell.
