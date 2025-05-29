# nrf_ble_dfu_dle
Adding DLE to nRF5340 simultaneous update for both cores to speed up DFU transfer times
An update to [this example](https://academy.nordicsemi.com/simultaneous-updates-for-both-cores-of-the-nrf5340/), which is an update to the [peripheral_lbs example](https://docs.nordicsemi.com/bundle/ncs-2.9.1/page/nrf/samples/bluetooth/peripheral_lbs/README.html) to add ota dfu for 5340 with simultaneous core update.

Recommend reviewing the following [LINK](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-9-bootloaders-and-dfu-fota/topic/dfu-for-the-nrf5340/
) for a refresher on the dfu process used in this repo
![image](https://github.com/user-attachments/assets/f7e176e4-5a95-4cd9-a569-343b6e36be3e)

## a refresher on DLE:

The data length and MTU (Maximum Transfer Unit) are two different parameters, but they often go hand in hand.

The MTU is the number of bytes that can be sent in one GATT operation (for example, a send operation), while data length is the number of bytes that can be sent in one Bluetooth LE packet. MTU has a default value of 23 bytes, and data length has a default value of 27 bytes. When MTU is larger than data length, such as MTU being 140 bytes while data length is 27 bytes, the data will be segmented into chunks of the data length’s size. This means that, for your application, it appears like one message is being sent, but on the air, the data is actually split into smaller segments

**Ideally, you want all of your data to be sent in one packet, to reduce the time it takes to send the data, so in Bluetooth 4.2, Data Length Extension (DLE) was introduced to allow the data length to be increased from the default 27 bytes to up to 251 bytes. Packing everything together also reduces the number of bytes you need to transmit over the air, as every packet includes a 3-byte header. This saves both time and power, and in turn allows for higher throughput in your Bluetooth LE connection.**

The relation between data length and MTU is not one-to-one. On air, the data length can be up to 251 bytes, while the actual payload that you can send is a maximum of 244 bytes. This is because the 251 byte Data PDU payload needs an L2CAP Header of 4 bytes, and an Attribute header of 3 bytes. This leaves you with 251 – 4 – 3 = 244 bytes that you can actually populate with payload data.

## No DLE iOS13 transfer speed 🐢

![image](https://github.com/user-attachments/assets/863e1275-e63d-4e2d-8efd-9f598f91586c)

## Add DLE! 🚀

![image](https://github.com/user-attachments/assets/ac7dcf08-f73a-42da-8615-0e722e8ad5c2)


# Requirements
## Hardware
- nRF5340DK
- iOS or Android device

## Software
- NCS v2.9.0/1
- Mobile Application: [nRF Connect Device Manager](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-Device-Manager)

# Instructions
- Flash with whatever `RUN_LED_BLINK_INTERVAL` you value. 
- Then change `RUN_LED_BLINK_INTERVAL` to something observably faster or slower (I like `1000` and `200` as values) and build again.
- **DO NOT** flash after building with the new interval. in the build/ directory you choose, obtain the dfu_application.zip and upload the image to your phone with nRF Connect Device manager.
- Observe the higher transfer speeds than the ones you saw without DLE. (You can comment out `update_data_length(my_conn);` and `update_mtu(my_conn);` if you want to have observably slower speed)
- (Optional) to avoid building with the modified blink rate, refer to the releases for .zips to play with.
