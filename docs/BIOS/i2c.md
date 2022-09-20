
## pin definition

![](https://www.analog.com/-/media/analog/en/landing-pages/technical-articles/i2c-primer-what-is-i2c-part-1-/36684.png?la=en&h=300&imgver=1)

  There are two lines(SDA, SCL) in i<sup>2</sup>c protocol, the SDA is used to send/receive data, it's biredirectional, and SCL is the clock.

  The priciple of data transfer in i<sup>2</sup>c protocol:  

  * IDLE: Both SCL and SDA are keep in high state.
  * SCL in high state, the data in SDA is valid and its value cannot be changed. The exceptions are:
    * START: SDA from '1' to '0'
    * STOP: SDA from '0' to '1'
  * SCL in low state, the state of SDA is updatable.

## Packet format


## Reference material
* [I2C的介紹](https://ithelp.ithome.com.tw/articles/10278308)