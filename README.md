# nrf_ble_dfu_dle
Adding DLE to nRF5340 simultaneous update for both cores to speed up DFU transfer times
An update to [this example](https://academy.nordicsemi.com/simultaneous-updates-for-both-cores-of-the-nrf5340/), which is an update to the [peripheral_lbs example](https://docs.nordicsemi.com/bundle/ncs-2.9.1/page/nrf/samples/bluetooth/peripheral_lbs/README.html) to add ota dfu for 5340 with simultaneous core update.

Recommend reviewing the following [LINK](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-9-bootloaders-and-dfu-fota/topic/dfu-for-the-nrf5340/
) for a refresher on the dfu process used in this repo
![image](https://github.com/user-attachments/assets/f7e176e4-5a95-4cd9-a569-343b6e36be3e)


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
