Ubidots-Dagino
===============

Ubidots-Dragino is an Dragino library for interacting with Ubidots through its API. 
The library contain, three LUA files and the library to Arduino IDE.
First you need two steps, install the library and paste the lua files in the dragino root folder


Install the Library
-------------------

1. Download the library as a ZIP file here

2. Open the Arduino IDE and go to "Sketch" --> "Import Library" --> "Add Library".

3. Select the downloaded ZIP file



Add the LUA files to dragino
----------------------------

1. Connect to dragino. In this wiki you can learn as http://wiki.dragino.com/index.php?title=Access_MS14

2. Go to the root directory and add the 3 LUA files.




A Quick example
----------------
Here's a quick example of how to use the library, with the serial terminal of Arduino:


.. code-block:: cpp

  /*
  Simple example to Dragino

  This sketch connects to Ubidots (http://www.ubidots.com) using an Dragino Hardware

  Is a simple example to explain the functions of the Dragino library


  Components:
  * Dragino Hardware
  * LUA files of Ubidots

  Created 5 Aug 2014
  by Mateo Vélez

  This code is in the public domain.

  */
	#include <ubidots_dragino.h>
	#include <Process.h>



	

	String api = "5ca9b10038e49e0492c6794f9043f0918ddcbd26";                    //your API key
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

Initialize a Ubidots client. This is most likely to be the first Ubidots library function to call.

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

Save a value to Ubidots. Returns true upon success. Returns false upon error.


get_value()
```````````
.. code-block:: cpp

    readvalue = ubiclient.get_value(idvari);

==================  ===========  =============================================
Type                Argument     Description
==================  ===========  =============================================
String              idvari       ID of the variable that you want make request 
==================  ===========  =============================================

Get value from Ubidots. Returns a String containing the last value of the variable.

ubitoken();
``````````` 

.. code-block:: cpp

    boolean = ubiclient.ubitoken(api);  

==================  ===========  =============================================
Type                Argument     Description
==================  ===========  =============================================
String              api          Your API key for the Ubidots API
==================  ===========  =============================================
