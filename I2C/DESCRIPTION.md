** I2C
inter-integrated circuit
  developed in 1982 by philips(now NXP)
  very common  
  primarily used for short distance data communications
Synchronous master-slave protocol
  both master and slaves can send/receive data
  bidirectional, half duplex
  can run at different speeds
Two wires: serial clock(scl) and serial data(sda)

Start condition
  in the idle state SDA and SCL are both high
  the start conditon occurs when a node
    first pull sda low
    then pulls scl low
  this claims the bus
    node is now the master
    prevents any other nodes from taking control of the bus
    reduces the risk of contention
  master that has seized the bus also starts the clock

Slave address
  Each i2c node on a bus must have a unique, fixed address
    normally 7 bit long, MSB first
    10 bit addresses also supported but these are uncommon
  address may be hard coded
  address may be partially configurable via external address lines or jumpers


Aside:timing relationship between SDA and SCL
  sda does not change between clock rising edge and clock falling edge
  during data transmission, sda only transitions while scl is low
    an sda transition when scl is high, indicates a start or stop condition

Read/write bit
  read/write bit follows the slave address
  set by master to indicate desired operation
    0-master wants to write data to slave
    1-master wants to read data from slave
  often interpreted and/or decoded as part of the address byte

ACK(knowledge) bit
  sent by the receiver of a byte of data
    0-acknowledgement (ACK)
    1-negative acknowledgement (NACK)
  Recall that i2c is idle high
    lack of response = NACK
  used after slave address and each data byte
  ACK after data byte(S) confirms receipt of data
  ACK after slave address confirms that
    a slave with that address is on the bus
    the slave is ready to read/write data(depending on R/W bit)

Data Byte(s)
  data byte contains the information being transferred between master and slave
      memory or register contents,address,etc.
  always 8 bit long, MSB first
  always followed by an acknowledgement bit
      set to zero by the receiver if data been recieved properly
    
Multiple data bytes
  in many cases, multiple data bytes are sent in one i2c frames
    each data byte is followed by ACK bit
  Bytes may be all data or some may represent an internal address,etc.
    ex -first byte is a register location and second byte is the data to be written to that register


Stop Condition
  stop condition indicates the end of data bytes
    first scl returns (and remains) high
    then sda returns (and remains) high
  recall that for data bytes, sda only transition when clock is low
    sda transition when scl high=stop condition
  bus becomes idle
    no clock signal
    any node can now use the start condition to claim the bus and begin a new communication


Open drain
  each line(sda and sdl) is connected to voltage(vcc or vdd) via a pull up resistor 
    one resistor per line(not per device)
  each i2c device contains logic that can open and close a drain
  when drain is closed the line is pulled low(connected to ground)
  when drain is open the line is pulled high (connected to voltage)
  i2c lines are high in idle state
      sometimes called an open drain system


Pull up resistor values
   pulling down a line is usually much faster than pulling up a line
     pull up time is a function of bus capacitance and values of pull up resistors
  values of pull up resistors are a compromise
    higher resistances increase the time needed to pull up the line thus limit bus speed
    lower resistances allow faster communications, but requires higher power
  Typical pull up resistor values are in the range of 1k ohm-10 k ohm.

  Modes/Speeds
    i2c can operate at different bus speeds
      referred to as "modes"
    table shows max speeds for each mode 
    i2c mode      speed
    standard       100kbps
    fast           400kbps
    fast plus      1mbps
    high speed      3.4mbps
    ultra fast      5mbps
      hardware is specified as compliant to standard,fast,or fast plus if it can(theoretically) achieve these speed
      High speed mode devices are backwards compatible to lower speeds
        transmit a special sequence to switch the bus to HS mode
      Utlra fast mode is unidirectional(write only)
        also requires modification to the protocol and frames.

  
    
   
  
    
    
  
  
  
  


  
