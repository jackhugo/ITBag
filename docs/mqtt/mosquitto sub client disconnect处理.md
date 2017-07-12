## 问题
sub client宕机重连后，如何能够接受到宕机期间的消息

## 解决方法
可以将sub client的clean session设为false，同时需要设置client id（broker能够标识同一个client）。

以下参见[mosquitto_sub官网](http://mosquitto.org/man/mosquitto_sub-1.html)

>-c, --disable-clean-session
Disable the 'clean session' flag. This means that all of the subscriptions for the client will be maintained after it disconnects, along with subsequent QoS 1 and QoS 2 messages that arrive. When the client reconnects, it will receive all of the queued messages.

>If using this option, it is recommended that the client id is set manually with --id

*注意*：这种方法只能对qos为1、2的消息有效。

### pub client和sub client都有qos，那他们之前又有什么关系？
sub的qos不会大于pub的qos，及时设置大于也无效。如：
- pub qos为2，sub qos可以设置为0、1、2
- pub qos为1，sub qos有效值为0、1
- pub qos为0，sub qos有效值为0

以下参见[mosquitto_mqtt协议](http://mosquitto.org/man/mqtt-7.html)

>MQTT defines three levels of Quality of Service (QoS). The QoS defines how hard the broker/client will try to ensure that a message is received. Messages may be sent at any QoS level, and clients may attempt to subscribe to topics at any QoS level. This means that the client chooses the maximum QoS it will receive. For example, if a message is published at QoS 2 and a client is subscribed with QoS 0, the message will be delivered to that client with QoS 0. If a second client is also subscribed to the same topic, but with QoS 2, then it will receive the same message but with QoS 2. For a second example, if a client is subscribed with QoS 2 and a message is published on QoS 0, the client will receive it on QoS 0.

>Higher levels of QoS are more reliable, but involve higher latency and have higher bandwidth requirements.

>0: The broker/client will deliver the message once, with no confirmation.

>1: The broker/client will deliver the message at least once, with confirmation required.

>2: The broker/client will deliver the message exactly once by using a four step handshake.
