# pythonawsiotcore
publishing data from python program to aws iot core

Connect to AWS Cloud 9 and upload the certificates

Add the below code

Python publish code
-------------------


from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTClient
import sys

myMQTTClient = AWSIoTMQTTClient("Test001IOT")

myMQTTClient.configureEndpoint("<thing-endpoint>", 8883)
myMQTTClient.configureCredentials("/home/ubuntu/environment/AmazonRootCA1.pem","/home/ubuntu/environment/private-certificate.pem.key", "/home/ubuntu/environment/device-certificate.pem.crt.txt")

myMQTTClient.connect()
print("Client Connected")

msg = "Sample data from the device";
topic = "general/inbound"
myMQTTClient.publish(topic, msg, 0)  
print("Message Sent")

myMQTTClient.disconnect()
print("Client Disconnected")


Subscribe code
--------------

import time

def customCallback(client,userdata,message):
    print("callback came...")
    print(message.payload)

from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTClient

myMQTTClient = AWSIoTMQTTClient("Test001IOT")
myMQTTClient.configureEndpoint("a3vot2fai8dlyg-ats.iot.ap-south-1.amazonaws.com", 8883)
myMQTTClient.configureCredentials("/home/ubuntu/environment/AmazonRootCA1.pem","/home/ubuntu/environment/private-certificate.pem.key", "/home/ubuntu/environment/device-certificate.pem.crt.txt")

myMQTTClient.connect()
print("Client Connected")

myMQTTClient.subscribe("general/outbound", 1, customCallback)
print('waiting for the callback. Click to conntinue...')
x = input()

myMQTTClient.unsubscribe("general/outbound")
print("Client unsubscribed") 


myMQTTClient.disconnect()
print("Client Disconnected")


==============================
Policy
======

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:*",
      "Resource": "*"
    }
  ]
}


