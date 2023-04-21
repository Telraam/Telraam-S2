# NRF testing procedure
# Tested firmware: 1.14

## 1. Up- and downgrade test on 1 device
1. Upload the new firmware to S3 (if that is not done already)
https://s3.console.aws.amazon.com/s3/buckets/telraam-sensor-v2-config?prefix=firmware_binaries/
Reame it to `nrf_x_yy.bin` before uploading.

2. Make sure your device is on the current production nrf firmware
Check the firmware config file: https://s3.console.aws.amazon.com/s3/object/telraam-sensor-v2-config?region=eu-central-1&prefix=firmware_config.json

3. Create a dynamic firmware config
Use the nrf firmware to test and the K210 fiwmare you want to use.
e.g.: `{"_comment_": "Test Carl", "NRF": "nrf_1_13.bin", "NRF_version": "1.13", "K210": "k210_binary_4_1_6.bin", "K210_version": 393476}` (put it in 1 line)
Save the file as `<device_id>.json`

4. Upload the dynamic firmware config to S3
https://s3.console.aws.amazon.com/s3/buckets/telraam-sensor-v2-config?prefix=firmware_config_dynamic/

5. Connect the device to your laptop to start monitoring the console logs

6. Start the console logging
Install the drivers first when this is not done already (https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=overview) .
`screen /dev/tty.usbserial-0001 115200`(tty device can be different)

7. The device should upgrade now, monitor the console logs for anything strange. 
 
8. Delete the dynamic config and reboot the device
https://s3.console.aws.amazon.com/s3/buckets/telraam-sensor-v2-config?prefix=firmware_config_dynamic/

9. The device should downgrade after the reboot

10. Repeat steps 2-6 to upgrade the device again to the NRF firmware that we are testing

###### Upgrade test: ok
###### Downgrade test: ok
&nbsp;
## 2. Deploy new FW to 8 devices for testing.
1. Create dynamic firmware configs for the 8 devices
Use the nrf firmware to test and the K210 fiwmare you want to use.
e.g.: `{"_comment_": "Test Carl", "NRF": "nrf_1_13.bin", "NRF_version": "1.13", "K210": "k210_binary_4_1_6.bin", "K210_version": 393476}` (put it in 1 line)
Save the files as `<device_id>.json`

2. Upload the dynamic firmware configs
https://s3.console.aws.amazon.com/s3/buckets/telraam-sensor-v2-config?prefix=firmware_config_dynamic/

3. Reboot the devices
By power cycle
Or send this message to the device:
`{
    "device": "352656104870798",
    "message": {
        "message-type": "debug-cmd",
        "reset": true
    }
}`

4. Monitor the devices and check whether they got upgraded

###### Devices upgraded: ok
&nbsp;
## 3. Test K210 up- and downgrade on the new NRF firmware.
1. Make sure the device is on the correct NRF firmware

2. Repeat steps 2 through 6 from the nrf up- and downgrade test, but now use an old and the current K210 firmware
`{"_comment_": "Carl's device", "NRF": "nrf_1_13.bin", "NRF_version": "1.13", "K210": "k210_binary_4_1_5.bin", "K210_version": 327940}`
 
###### Upgrade test: ok
###### Downgrade test: ok
&nbsp;

## 4. Check for message functionality (hello, config update loop)
1. Make sure the device is on the correct NRF and K210 firmwares

2. Reboot the device
 
3. Check the hello message being sent on Thingstream
Search for the  device at https://portal.thingstream.io/app/communication-services/things , click on the device and then on 'traffic logs'

4. Also check the config-report being sent on Thingstream
Search for the  device at https://portal.thingstream.io/app/communication-services/things , click on the device and then on 'traffic logs'

5. Check the hello message on aws athena
Go to https://eu-central-1.console.aws.amazon.com/athena/home?region=eu-central-1#/query-editor/
And execute `select *
from "telraam"."online_notification"
where cast("device-id" as bigint)=352656104870798
order by "timestamp" desc`

6. Check the config-report in the lambda logs
Go to https://eu-central-1.console.aws.amazon.com/cloudwatch/home?region=eu-central-1#logsV2:log-groups/log-group/$252Faws$252Flambda$252FSv2-config-report and filter on your device-id

7. Send a config update to the device, set gps-acq-time to 5 (this key is not used anymore, but we can use it for testing)
`{
    "device": "352656104870798",
    "message": {
        "message-type": "config-update",
        "gps-acq-time": 5
    }
}`

8. Check on Thingstream that the message is sent to the device
Search for the  device at https://portal.thingstream.io/app/communication-services/things , click on the device and then on 'traffic logs'

9. Check on Thingstream that the device replies with a full config-report, and that gps-acq-time is set to 5
Search for the  device at https://portal.thingstream.io/app/communication-services/things , click on the device and then on 'traffic logs'

10. Reset gps-acq-time to 10
`{
    "device": "352656104870798",
    "message": {
        "message-type": "config-update",
        "gps-acq-time": 10
    }
}`

###### hello-messaage: ok
###### config-report: ok
###### config-update: ok
&nbsp;

## 5. Execute functionality test
### 5.1 Let the device boot in debug mode -> should be all green
1. Reboot the device and hold down the button

2. Let the device count traffic
Put it on a window or wave it a bit around
After 8 counts, the automatic ROI should be found
After 10 counts, the traffic count should be ok

2. Check the screen
Status should be 'SUCCESS'
###### status is 'SUCCESS': ok
&nbsp;


### 5.2 Test messaging functionality
1. Send a skippable message to the device
https://telraam-api.net/v1/private/s2/message
`{
  "device-id": 352656104870798,
  "error-message": "~~This is a test message.~~It should be skippable.",
  "default-message": false,
  "user-skippable": true
}`

2. Check the device screen for the message.

3. This message should be skippable.

4. Clear the message.
https://telraam-api.net/v1/private/s2/message
`{
  "device-id": 352656104870798,
  "error-message": "",
  "default-message": false,
  "user-skippable": true
}`

5. Check that the device screen is cleared.

6. Send a non-skippable message to the device
https://telraam-api.net/v1/private/s2/message
`{
  "device-id": 352656104870798,
  "error-message": "~~This is a test message.~~It should NOT be skippable.",
  "default-message": false,
  "user-skippable": false
}`

7. Check the device screen for the message.

8. This message should not be skippable.

9. Clear the message, see step 4. 


###### skippable message: ok
###### clearing message: ok
###### non-skippable message: ok
&nbsp;

### 5.3 See if it counts (counts are in screen and in database)
1. Put the deive on a window

2. After some traffic has passed, check the counts on the screens on the device

3. Check the database for the counts
`select * from telraam.aggregate_info
where device_id=352656104870798
and date>now()-interval '2 hours'
order by date desc`

###### counts on the screen: ok
###### counts in the database: ok
&nbsp;

### 5.4 Request a picture
1. Request a picture via the lambda function
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/Sv2-picture/aliases/prod?tab=testing
`{
  "device-id": 352656104870798
}`

2. Check whether the picture arrived at the S3 storage
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

###### picture arrived on S3: ok
&nbsp;

### 5.5 ROI check with API: set 0 first and then set something else (request images too).
1. Mak sure auto roi selection has finished

2. Set ROI=0 and request image
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": 0,
  "request-image": true,
  "update-config": false
}`

3. Check image arrived in the S3 storage
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

4. Set ROI to someting else
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": 9,
  "request-image": true,
  "update-config": false
}`

5. Check the image with the new ROI
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

###### ROI was set to 0: ok
###### ROI was set to something else: ok
###### pictures arrived on S3 and had different zoom levels: ok
&nbsp;

### 5.6 ROI 666 test (send 666 and boot to debug mode to see auto selection works)
1. Set ROI to auto selection
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": 666,
  "request-image": false,
  "update-config": false
}`

2. Boot into debug mode
Hold down the button during startup

3. Let the device count traffic (at least 10) to see whether the ROI=666 gets updated to something else

###### ROI was set to 666: ok
###### ROI auto selection in debug mode: ok
&nbsp;

### 5.7 Set ROI to something fixed
1. Fix ROI to something
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": 9,
  "request-image": false,
  "update-config": true
}`

2. Check the console logs

3. Request an image
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": -1,
  "request-image": true,
  "update-config": false
}`

5. Check the image with the new ROI
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

6. Reboot the device

7. Check the console logs for the fixed ROI to be applied

8. Request an image
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": -1,
  "request-image": true,
  "update-config": false
}`

9. Check the image with the new ROI
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

10. Reset the ROI to 666
Delete the region-of-interest key from the dynamic config
Upload the dynamic config
Reboot the device
Check the console logs for `Launched automatic ROI search`

###### image had the new ROI: ok
###### fixed ROI still fine after reboot: ok
###### ROI reset to 666: ok
&nbsp;

### 5.8 Test if we can point the device to our test environment
1. Send a config-update to the device pointing it to the test environment
`{
    "device": "352656104870798",
    "message": {
        "message-type": "config-update",
        "debug-allowed": true
    }
}`

2. Check in the console logs that the message arrives on the device

3. Check in the console logs that the devices reconnects the LTE link
This is necessary because the devices will reconnect to Thingstream and will subscribe to a different topic 

4. After an automatic config-report and config-update cycle, the device should go back to the prod environment

4. Open the page with the versions of the Sv2-hello lambda
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/Sv2-hello?tab=versions

5. Filter the logs for your device and check that the test lambda version was used
https://eu-central-1.console.aws.amazon.com/cloudwatch/home?region=eu-central-1#logsV2:log-groups/log-group/$252Faws$252Flambda$252FSv2-hello

###### device was pointed to the test environment: ok / nok 
&nbsp;

### 5.9 Check if we can reboot the device remotely
1. Send this message to the device
`{
    "device": "352656104870798",
    "message": {
        "message-type": "debug-cmd",
        "reset": true
    }
}
`
2. Check if the device has rebooted

###### device has rebooted: ok
&nbsp;

### 5.10 Check if ROI is stable.
1. Fix ROI to something
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": 9,
  "request-image": true,
  "update-config": true
}`

2. Check the image with the new ROI
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

3. Power cycle the device

4. Request an image 
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": -1,
  "request-image": true,
  "update-config": false
}`

5. Check the image
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

6. Reboot the device via the api, send this message to the device
`{
    "device": "352656104870798",
    "message": {
        "message-type": "debug-cmd",
        "reset": true
    }
}
`

7. Request an image 
https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/functions/api-Sv2-set-and-get-ROI/aliases/prod?tab=testing
`{
  "device-id": "352656104870798",
  "ROI": -1,
  "request-image": true,
  "update-config": false
}`

8. Check the image
https://s3.console.aws.amazon.com/s3/buckets/telraam-street-pictures?region=eu-central-1&tab=objects

9. Delete the dynamic config
https://s3.console.aws.amazon.com/s3/buckets/telraam-sensor-v2-config?prefix=config_dynamic%2F&region=eu-central-1#

###### ROI ok after power cycle: ok
###### ROI ok after reboot via api: ok
&nbsp;

## 6 Let the 8 devices run for 3 days to see if everything is still fine (not more reboots than expected, no data loss, etc.)
1. Check the devices for reboots in athena
Go to https://eu-central-1.console.aws.amazon.com/athena/home?region=eu-central-1#/query-editor/
And execute `select *
from "telraam"."online_notification"
where cast("device-id" as bigint) in (
350457790597577,
350457790598740,
350457790597650,
350457790626681,
350457790597676,
350457790600256,
350457790600793,
350457790600603
)
order by "device-id", "timestamp" desc`

2. Check if devices kept sending count data to the DB
`select device_id, extract(epoch from date) as epoch,*
from telraam.aggregate_info
where device_id in (
350457790597577,
350457790598740,
350457790597650,
350457790626681,
350457790597676,
350457790600256,
350457790600793,
350457790600603
)
and period='quarterly'
and segment_id=-1
and date>now()-interval '3 days'
order by device_id,date`
Check for gaps in the dates and bad uptime (pct)
###### reboots: ok
###### counts: ok
&nbsp;

## 7 Full firmware test result
###### Full firmware test result: ok
Everything was ok, the bug in the ROI behaviour was fixed.
&nbsp;
