Ubidots-Dragino
===============

Ubidots-Dragino is a library to interact with the Ubidots API using Dragino MS14 products
(http://www.dragino.com/products/mother-board.html) in conjunction with its "Arduino IoT Modules" (http://www.dragino.com/products/module.html).

The Dragino is a versatile board that works as a gateway for most IoT applications. It ships with built-in WiFi and can be
programmed using the Arduino IDE.

This library contains three LUA files and the library to use with the Arduino IDE.

First you'll need to install the Dragino library in your Arduino IDE and then paste the lua files in Dragino's root folder:

Components
----------

* `Dragino MS14 System Structure <http://www.dragino.com/products/mother-board/item/72-ms14-s.html>`_:

* `M32W wireless Internet of Things module <http://www.dragino.com/products/module/item/84-m32w.html>`_:

* 3 Lua files, those files are to get a token and post and get a value of your variable. 

   *`Post Lua file <https://github.com/ubidots/ubidots-dragino/blob/master/Content/ubidots_post.lua>`_
   *`Get Lua file <https://github.com/ubidots/ubidots-dragino/blob/master/Content/ubidots_get.lua>`_
   *`API token Lua file <https://github.com/ubidots/ubidots-dragino/blob/master/Content/ubidots.lua>`_

Install the Library
-------------------

1. `Download the library as a ZIP file <https://github.com/ubidots/ubidots-dragino/blob/master/Content/dragino.zip?raw=true>`_

2. Open the Arduino IDE and go to "Sketch" --> "Import Library" --> "Add Library".

3. Select the downloaded ZIP file



Add the LUA files to Dragino
----------------------------

1. Connect your dragino to the power supply.

2. Check there will be an unsecure wifi network start with Arduino Yun or Dragino - xxxxxxxxxxxx show in wifi connection.

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_wifi.png
    :name: dragino-wifi
    :align: center
    :alt: dragino-wifi

3. Connect to this network and so we can enter 192.168.240.1 into the broswer and see the configure page of ms14. The default pass is "arduino".

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_pass.jpg
    :name: dragino-pass
    :align: center
    :alt: dragino-pass

4. Once you are connected press "system".

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_system.jpg
    :name: dragino-system
    :align: center
    :alt: dragino-system

5. Connect dragino to your SSID and press "configure & restart". This may take several minutes.

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_config.jpg
    :name: dragino-config
    :align: center
    :alt: dragino-config

6. Now you can see in the port tab of the Arduino IDE (1.5.7) the dragino with the IP. Takes note of this IP.

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_ip.jpg
    :name: dragino-ip
    :align: center
    :alt: dragino-ip

7. Open WinSCP and place the IP that scored before, use SCP protocol, User: root and pass: arduino.

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_winscp.jpg
    :name: winscp
    :align: center
    :alt: winscp

8. Now add the lua files in the root directory. 

.. figure:: https://github.com/ubidots/ubidots-dragino/blob/master/Content/yun_root.jpg
    :name: dragino-root
    :align: center
    :alt: dragino-root




A Quick Example
----------------
Here's a quick example of how to use the library, with the serial terminal of Arduino:


.. code-block:: cpp

  /*
  Simple example to Dragino

  This sketch connects to Ubidots (http://www.ubidots.com) using a Dragino Hardware

  This is a simple example to explain the functions of the Dragino library


  Components:
  * Dragino Hardware
  * LUA files of Ubidots

  Created 5 Aug 2014
  by Mateo Vélez

  This code is in the public domain.

  */
	#include <ubidots_dragino.h>
	#include <Process.h>



	

	String api = "5ca9b10038e49e0492c6794f9043f0918ddcbd26";                    //your Ubidots API key
	String idvari = "53badbd77625425f8665d11b";                                 //this is de ID of the variaable that do you want to know
	Ubidots ubiclient(api);                                                     //initializes the Ubidots library 

	void setup() {
	  // start serial port:
	  Bridge.begin();                                                           //initializes the bridge library
	  Console.begin();                                                          //initializes the console

	  while (!Console);                                                         //wait for Network Serial to open
	  Console.println("Ubidots client");                                        
	  ubiclient.ubitoken(api);                                                  //get the token to the communication with Ubidots
	  delay(20);
	  
	}

	void loop() {
	 
	  int incomingByte = 678;                                                   //a random value for post it to Ubidots
	  String idvari2= "53baaf3c76254244e1c8e408";                               //the ID of the variable that do you want to post
	  String valor;                                                             //a random variable to save the value after that you ask to the Ubidots API
	  valor = ubiclient.get_value(idvari);                                      //the function to get the value of your variable, return the value
	  Console.println(valor);
	  ubiclient.save_value(idvari2,String(incomingByte));                       //the function to save value in your variable
	}


API Reference
-------------

Ubidots ubiclient():
````````````````````
.. code-block:: cpp

    Ubidots ubiclient(api);

=======  ========  =================================
Type     Argument  Description
=======  ========  =================================
String   api       Your API key for the Ubidots API
=======  ========  =================================

Initializes a Ubidots client. This is most likely to be the first Ubidots library function to call.

save_value()
````````````````````
.. code-block:: cpp

    boolean = ubiclient.save_value(idvari, String(incomingByte), ctext)
=======  ============  ===================================
Type     Argument      Description
=======  ============  ===================================
String   idvari        ID of the variable to save
String   incomingByte  The value of the sensor
=======  ============  ====================================

Saves a value to Ubidots. Returns true upon success. Returns false upon error.


get_value()
```````````
.. code-block:: cpp

    readvalue = ubiclient.get_value(idvari);

==================  ===========  =============================================
Type                Argument     Description
==================  ===========  =============================================
String              idvari       ID of the variable that you want make request 
==================  ===========  =============================================

Gets value from Ubidots. Returns a String containing the last value of the variable.

ubitoken();
``````````` 

.. code-block:: cpp

    boolean = ubiclient.ubitoken(api);  

==================  ===========  =============================================
Type                Argument     Description
==================  ===========  =============================================
String              api          Your API key for the Ubidots API
==================  ===========  =============================================
