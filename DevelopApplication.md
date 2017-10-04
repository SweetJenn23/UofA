#Developing a blockchain application and front end sample

In this exercise, you will import the components of a Hyperledger Composer to create an application, generate API for the deployed application and then integrate a front end created in Node-RED to interat with the blockchain network.

#### Importing the components of your blockchain application

1. In a terminal on your computer, move to the home directory. `cd $HOME`
2. If not already installed, [install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git_) for your computer. 
3. Once Git is installed, run the following command to clone the needed materials for this exercise. `git clone https://github.com/IBM/HyperledgerFabric-on-LinuxOne.git`
  ![Clone GitHub repository](images/CloneGitHubRepo.png)
4. To find the files you'll need for this, `cd HyperledgerFabric-on-LinuxOne/code/` and then run `ls` to see what is in the directory.
  ![Move to code directory.](images/MoveToCodeDir.png)
5. Enter `pwd` to see where you are on your system. Save this information. You'll need it in a few steps.
  ![Enter pwd.](images/pwd.png)
6. Go back to your browser that has Composer Playground open. If you've closed it, you can open it in your browser by entering `xxx.xxx.x.x:8080` into the address bar where the x's correspond to your Linux guest's IP address.
   - **Note:** You will need to view the browser in Full Screen (fully expanded) mode to be able to access everything and prevent issues with inability to scroll on certain screens.
     ![View Composer Playground.](images/ComposerPlaygroundUI2.png)
7. Select **Deploy a new business network**.
   ![Select Deploy a new business network.](images/SelectImportReplace.png)
8. Complete the *BASIC INFORMATION*.

   - Give your new Business Network a name: **blockchain-journery**

   - Describe what your Business Network will be used for: **Creating my first blockchain network.**
     ![Complete the Basic Information](images/basicinfo.png)
9. Scroll until you can see *Choose a Business Network Definition to start with:* and select **empty-business-network** and **Deploy**.
   ![Click empty-business-network and Deploy.](images/EmptyBusinessNetwork.png)
10. - From *My Wallet* select **Connect now** to go into your business network.
    ![Select Connect now.](images/ConnectNow.png)
11. Select **Add a File**.
   ![Select Add a File.](images/AddFile.png)
12. From the *Add a file* pop-up dialog, select **browse**.
   ![Select browse.](images/SelectBrowse.png)
13. In the file explorer window, navigate to where you downloaded the files. Refer to step 5 if you need help finding this location. **Select** *README.md* and **Click** *Open*.
   ![Select README.md](images/SelectREADME.png)

14. **Select** *Add*.
    ![Select Add.](images/AddREADME.png)

15. On the *Current file will be replaced* dialog, **select** *Replace*.
   ![Select Add.](images/READMEReplace.png)

16. Let's keep adding the files to the Composer Playground.  **Repeat steps 11-15 to add the following files**:

- *org.acme.sample.cto* — This is located in the models folder. In this exercise you'll use this file to create a model for your asset and transactions. You could also create participants in this file. This is similar to creating a Java class and defining what you would need in the class.
- *logic.js* — This is located in the lib folder. This is a JavaScript file that becomes the brains of your application. In this file is code, your smart contract, that defines how a transaction can happen. This is similar to Java methods.
- **Add last:** *permissions.acl* — This is where you would limit permissions for participants in a blockchain network.

17. Your files are all now loaded into Composer Playground. **Click** *Update* on the left side of the browser. 
   ![Click Deploy.](images/InitialDeploy.png)

#### Creating your blockchain application

18. Click on **Model File**.
   ![Click Model File](images/SelectModelFile.png)

19. Click in the **editor** on the right to begin writing your models. 

    - NOTE: **DO** **NOT** modify the namespace during the lab.
      ![Click in the editor](images/ClickEditor.png)

20. On a new line, give your asset `Sensor` the following attributes.

    - Note: a small "o" is used as a bullet in the model.

    - `o String teamID` — this will be the value that is assigned to your team. (already there!)

    - `o String teamName`— this could be anything! Come up with something clever!

    - `o Double sensorTemp` — temperature from the Raspberry Pi will be stored here.

    - `o Double thermostatTemp`— you will create a temperature for the thermostat.

    - `o String recommendation`— this will be populated based on the `CompareWeather` transaction.

    - **Click** *Update* to save changes.
      ![Sensor model](images/SensorModel.png)

21. Now create your first transaction model for `SetSensorTemp`. Enter the following attributes:

    - `--> Sensor gauge` — The transaction will need to put data into the `Sensor` asset. This passes a reference to the asset so we can work with the asset in the logic for the transaction.

    - `o Double newSensorValue` — This is the variable that will be set by the temperature passed into the transaction from the NodeRed Sensor for picking up temperature.

    - *Click** *Deploy* to save changes.

      ![Create SetSensorTemp model](images/SetSensorTempModel.png)

22. Build your `ChangeThermostatTemp` transaction model. Add the following:

    - `--> Sensor thermostat` — The transaction will need to put data into the `Sensor` asset for the thermostat. This passes a reference to the asset so we can work with the asset in the logic for the transaction.

    - `o Double newThermostatValue` — This allows for a new, proposed value to be sent into the transaction. In the logic tab, we will use this value to compare to what the gauge says and decide if the thermostat value should be adjusted.

    - **Click** *Update* to save changes.

      ![Create ChangeThermostatTemp model](images/ChangeThermostatModel.png)

23. Enter the following values to build your `CompareWeather` transaction model:

    - `--> Sensor recommend` — The transaction will need to put data into the `Sensor` asset. This passes a reference to the asset so we can work with the asset in the logic for the transaction.

    - `o Double outsideTemp` — Looking at the [WeatherUnderground.com API](https://www.wunderground.com/weather/api/d/docs?d=data/conditions) for Conditions, you can see all of the possible data that the call could return. Based on the data, it was decided to take the actual outside temperature and the feels like temperature to give a recommendation on thermostat settings. This variable stores the value passed into it via NodeRed from Weather.com for the outside temperature.  The model on the API page shows up whether the data is returned in Celsius or Fahrenheit and its variable type. In this exercise we will use Celsius.

    - `o Double feelsLike`— the variable to store the feels_like value from Weather.com.

    - **Click** *Update* to save changes.
      ![create CompareWeather model](images/CompareWeatherModel.png)

24. Click on the **Script File** tab.
   ![Click Script File](images/ClickScriptFile.png)

25. **Review the code in the editor. **Verify that your variable names match the variable names here.  Capitalization does matter! If names don't match, you'll have errors. 

    - Any guesses what the code is doing for each transaction?
      ![Review code](images/ReviewScriptFile.png)

    #### Test application code

26. Click on the **Test** tab at the top to try out your code.
   ![Click Test](images/ClickTest.png)

27. Notice that in this particular case because we have no participants, the **Test** tab has opened to the **Asset** menu on the left. You must have an asset to be able to run any of the transactions.

    - Click **Create New Asset**.

      ![Click Create New Asset](images/CreateNewAsset.png)

28. Create an example asset to test your code by filling in the following information:

    - `"teamID": "teamID:**xxx**"` where ** **xxx** ** is any team number you'd like.

    - `"teamName":""` — this could be any name you'd like. Be clever! :bowtie:

    - `"sensorTemp": **0**` — Set ** **0** ** to any value. When you work with NodeRed, temperatures will be in Celsius.

    - `"thermostatTemp": **0** `— Set ** **0** ** to any value. This is initializing your thermostat so pick a value you want to work with.

    - `"recommendation": "" `— Leave this as is.

    - *Make a note somewhere** of the values you used for `sensorTemp` and `thermostatTemp`.

      ![Create asset](images/NewAssetValues.png)

29. Click **Create New**.

    ![Click Create New](images/ClickCreateNew.png)

30. Once your **Team** asset is created it should show in the registry as shown below.

    ![Asset registry](images/Team01Asset.png)

31. You're ready to run your first transaction. **Click** on *Submit Transaction*.

    ![Click Submit Transaction](images/ClickSubmitTransaction.png)

32. The **Submit Transaction** dialog will open a new window. 

    - Make sure that the **Transaction Type** is set to `SetSensorTemp`.

    - Modify the JSON data`"gauge": "resource:org.acme.sample.Sensor#teamID:xxx"`  — enter your team's identifier in place of the value where **xxx** is in the sample JSON data.

    - Modify the JSON data`"newSensorValue": 0` — enter a value your sensor could have.

    - Click **Submit**.

      ![Submit SetSensorTemp](images/SetSensorTempTran.png)

33. If you submitted the transaction with your correct team ID, then you should have a transaction showing in your registry with the data you entered in the prior step. Congratulations! You've now completed a transaction. 👍

    ![Transaction Registry](images/TransactionRegistry.png)

34. Verify that `SetSensorTemp` updated the `sensorTemp`value in your asset. Click **Sensor**.

    ![Click Team](images/ClickSensor.png)

35. Check the `sensorTemp` value. Does it have the new value from the `SetSensorTemp` transaction?

    ![Check sensorTemp value](images/VerifySensorTemp.png)

36. Let's do another transaction. Select **Submit Transaction**.

   ![Select Submit Transaction](images/SubmitTransaction2.png)

37. This time let's run, `ChangeThermostatTemp`. 

    - In the **Transaction Type** drop down, select `ChangeThermostatTemp`.
      ![Select ChangeThermostatTemp](images/SelectChangeThermostat.png)

    - Edit the sample JSON for the transaction`"thermostat": "resource:org.acme.sample.Sensor#teamID:xxx"`— change **xxx** to your team ID value.

    - Edit the sample JSON for the transaction`"newThermostatValue": 0` — Replace **0** with a value to which you would like to see if you can adjust the thermostat.

    - Click **Submit**.

      ![Submit ChangeThermostatTemp](images/ChangeThermostatTran.png)

    - If you select a temperature for the thermostat that is not within 3 degrees of the `sensorTemp` value, then you will get an error message like the one below. If you get this message, enter another value and click submit.

      ![ChangeThermostatTemp Error Message](images/ChangeThermostatError.png)

    - If you do have permission to adjust the thermostat, you will be returned back to the transaction registry where you can see the data you just submitted.

      ![Successful Transaction](images/TransactionRegistry2.png)

    - If for some reason you forget to modify your teamID value or update it to the wrong value, you will see an error like the one shown below. Check your value for teamID and try again.

      ![Asset does not exist error message](images/TeamIDError.png)

38. Verify that the last transaction updated your asset. Click **Sensor**.

    ![Click Sensor](images/ClickSensor2.png)

39. Verify that the `thermostatTemp` attribute for your Team has been updated to the value you gave successsfully in the `ChangeThermostatTemp` transaction.

    - **Note**: In step 40, you can verify that the thermostat was originally set to 20 and is now set to 16.

      ![Verify thermostatTemp value](images/VerifyThermostatTemp.png)

40. Time to work with the `CompareWeather` transaction. Click **Submit Transaction**.

    ![Click Submit Transaction](images/SubmitTransaction3.png)

41. Select **CompareWeather** from the *Transaction Type* drop down.

    ![Select CompareWeather](images/Part1_Step36.png)

42. Complete the **CompareWeather** transaction.

    - Modify the JSON, `"recommend": "resource:org.acme.sample.Sensor#teamID:xxx"`— Replace **xxx** with your team ID.

    - Modify the JSON for`"outsideTemp": 0`— Enter a value for an outside temperature.

    - Edit the JSON for`"feelsLike": 0` — Enter a value for what temperature it could feel like outside.

    - Click **Submit**.

      ![Complete CompareWeather](images/CompareWeatherTran.png)

43. Verify that your transaction is showing in the Transaction Registry.

    ![Transaction Registry](images/TransactionRegistry3.png)

44. Click on **Sensor**. 

    ![Click Sensor](images/ClickSensor3.png)

45. Verify there is now a message in the `recommendation`variable in your Team asset and that the `thermostatValue` has been updated to the recommended value.

    ![Team asset recommendation value](images/VerifyRecommendation.png)

46. Continue testing your code for all scenarios to understand what your contract(s) can do. The hints to the remaining scenarios are as follows: (Yes, you'll have to look at the Script File under the Define Tab to figure out the criteria.)

    - ChangeThemostatTemp:
      - [ ] A successful transaction where the `thermostatValue` is updated in the Sensor asset.
      - [ ] An error message in the *Submit Transaction* window advising you do not have permission to adjust the thermostat.
    - CompareWeather:
      - [ ] A transaction based on `outsideTemp` values where it is really hot.
      - [ ] A transaction based on `outsideTemp` values where it is quite nice.
      - [ ] A transaction based on `outsideTemp` values where it is cold.
      - [ ] A transaction based on `feelsLike` values where it hot.
      - [ ] A transaction based on `feelsLike` values where it is quite nice.
      - [ ] A transaction based on `feelsLike` values where it is cold.
    - **Note:** You should verify that your asset values have been updated appropriately after each transaction like you did in prior steps.

#### Deploy application to Hyperledger Fabric

47. In your terminal connected to your Linux guest, enter the command `cd ~/.composer-connection-profiles/`. Enter `ls` to see the profiles in the directory. The profile was created during the setup script. You'll need the information in it to connect Hyperledger Composer to Hyperledger Fabric.
   ![View your connection profiles.](images/ComposerConnectionProfile.png)

48. Move into the profile directory, `cd hlfv1` and view the file in it by doing `cat connection.json`. Keep the terminal available, you'll need to view this information in just a second.

    ![View connection.json](images/ConnectionJSON.png)

49. Back in your browser where Hyperledger Composer Playground is running, **click** the *Define* tab and then **click** *Export* to save your code to your desktop. This is a safety measure. Export saves all of the indivudual files we imported at the beginning of Part 2 into a compressed file called a business network archive (.bna).

    ![Click Export](images/ClickExport.png)

50. In the pop-up dialog, **click** *Save File*.

    ![Click Save File.](images/SaveFile2.png)

51. In the upper right corner of your browser, **select**  *admin* and **click** *logout*.

    ![Select admin and logout.](images/ClickLogout.png)

52. In the right corner, **Select** *Create ID card*.

    ![Select Create ID card.](images/CreateID.png)

53. On the *Create ID Card* dialog, **select** *Hyperledger Fabric v1.0* and **click** *Next*.

    ![Select Hyperledger Fabric v1.0.](images/SelectHLFv1.png)

54. Complete the following fields according to the information in your connection.json on the Linux guest. This is in your terminal as found in step 49. **Information for Orderer, Channel, MSP ID, CA, Peers and Key Value Store must be exact.**

    - Connection Profile — LinuxONECC

    - Orderer(s) — `grpc://localhost:7050`

    - Channel — `composerchannel`

    - MSP ID — `Org1MSP`

    - CA — `http://localhost:7054`

    - Peer(s) — `grpc://localhost:7051`, `grpc://localhost:7053`

    - Key Value Store — `/home/linux1/.composer-credentials`

    - **Click** *Next*.

      ![Enter the information in the Composer Playground Profile.](images/PlaygroundConnectionProfile.png)

55. In the *Create ID Card* dialog, complete the following: 

    - **Select** *ID and Secret*.
    - **Create** an *Enrollment ID* of `PeerAdmin`.
    - **Create** an *Enrollment Secret* of `linux`.
    - **Select** *Admin Card* for the Card Type.
    - **Select** *Peer Admin* and *Channel Admin* for the Role.
    - **Click** *Create*.

    ![Create the credentials for the ID card.](images/IDCardCreds.png)

56. Under the *Identity cards for LinuxONECC*, **click** *Deploy a new business network*.

   ![Click Deploy a new business network.](images/LinuxONEDeploy.png)

57. Under *Basic Information*, **enter** a business network name of `journey-deploy`.

   ![Fill in the values.](images/journeydeploy.png)

58. Navigate to wherever you saved your `blockchain-journey.bna` in step 51, **select** `blockchain-journey.bna` and **click** *Open*.

    ![Deploy bna.](images/DeployBlockchainJourney.png)

59. Review the summary of your selections and **click** *Deploy*.

    ![Click Deploy.](images/ClickDeploy1.png)

60. In your wallet, you should now see a new identity card for `journey-deploy` under *Identity cards for LinuxONECC*.

    ![Deployed journey-deploy.](images/DeployedJourney.png)

61. Back in your terminal, enter `docker ps -a` . You can see there is now a new container running where Composer Playground has deployed code to the Hyperledger Fabric.

    ![View Hyperledger Composer Playground container.](images/PlaygroundContainer.png)

62. Switch back to your Hyperledger Composer Playground browser, **click** *Connect now*.

    ![Click Connect now.](images/ConnectNow1.png)

63. You should see your journey-deploy in the Hyperledger Composer Playground when it is successfully deployed. Congratulations! You've deployed your first blockchain application to Hyperledger Fabric.

    - **Note:** As a safety measure, check the Model File and Script File to make sure your code is there. It is also good practice to run some transactions in the test tab.

    ![Successful deployment.](images/SuccessfulDeployBNA.png)

    #### Generating API

64. In your terminal, issue the following commands to start the API rest server:

    - `mkdir /data/linux1/playground`

    - `nohup composer-rest-server -p hlfv1 -n journey-deploy -i PeerAdmin -s linux -N always >/data/linux1/playground/rest.stdout 2>/data/linux1/playground/rest.stderr & disown`
      ![Start your API rest server.](images/StartRestServer.png)

65. Verify the rest server process is running. `ps -ef|grep rest`
   ![Verify the rest server is running.](images/VerifyRestServer.png)

66. To see your API, go back to your browser and open a new tab or window. In the address bar, enter `http://xxx.xxx.x.x:3000/explorer` where the x's are the IP address for your Linux guest. You should see a page like the one shown.
   ![View your REST APIs.](images/RestAPI.png)

67. Expand the different methods to see the various calls and parameters you can make through REST API. You can also test the API in this browser to learn how to form the API and see the responses.
   ![Test your API.](images/TestAPI.png)

68. Congratulations! You now have a working blockchain application and have created APIs to call your blockchain application.

### Utilizing blockchain API through NodeRED

#### Importing your flow into NodeRED

69. Open NodeRED in your browser in a new tab or window. Enter `http://xxx.xxx.x.x:1880` in the address bar where the x's correspond to your Linux guest IP address.
   ![Open NodeRED.](images/OpenNodeRED.png)

70. You'll need to add a few more nodes to your NodeRED palette to have complete working flows. To do this, **select** the *menu* button in the upper right corner.
   ![Select the menu icon.](images/node-red-menu.png)

71. **Select** *Manage Palette* from the drop down menu.
   ![Select Manage Palette.](images/ManagePalette.png)

72. In the *User Settings* window, **click** *Install*, **type** `dashboard` in the search bar and select **install** next to *node-red-dashboard*.
   ![Install node-red-dashboard nodes.](images/install-node-red-dashboard.png)

73. In the *Install nodes* pop-up, **click** *Install*.
   ![Click install.](images/install.png)

74. Now it's time to install a RaspberryPi Sense Hat simulator. In the *User Settings* window under *Install*, **type** `sense` in the search bar and select **install** next to *node-red-pi-sense-hat-simulator*.
   ![Install Raspberry Pi Sense Hat Simulator nodes.](images/PiNodes.png)

75. In the *Install nodes* pop-up, **click** *Install*.
   ![Click install.](images/install.png)

76. **Click** *Close* to leave the User Settings dialog.
   ![Click Close.](images/closePalette.png)

77. **Copy** all of the JSON from the [GitHub repository](https://raw.githubusercontent.com/IBM/HyperledgerFabric-on-LinuxOne/master/code/node-red.json) to import into the flow. 
    - **Note:** The easiest way to do this is clicking the hyperlink above. You can also find it in the GitHub repository in the code folder as `node-red.json`. To view it in GitHub, **click** on *node-red.json*, **select** *raw* and then copy it. 
      ![Copy the raw JSON.](images/RawJson.png)

78. Paste it into NodeRed, by **clicking** on the *menu icon* in the upper right corner.
   ![menu](images/node-red-menu.png)

79. **Select** *Import -> Clipboard*.
   ![menu](images/node-red-menu-import-clipboard.png)

80. **Paste** the code in the editor. Make sure to **select** "current flow" button. **Select** *Import*.
   ![import editor](images/node-red-menu-import-editor.png)

81. You should now have three flows in NodeRED — Blockchain, Dashboard and Gauge.
   ![Three NodeRED flows.](images/ThreeFlows.png)

82. On the right side of the Node-RED browser, **click** *Deploy* to save the work you've imported.
   ![Click Deploy.](images/ClickDeploy.png)


#### Interacting with blockchain through a dashboard

83. **Click** the *dashboard* tab in the upper right corner under *Deploy*.
   ![Click dashboard.](images/ClickDashboard.png)

84. **Click** the *pop out* button to open the dashboard in a browser.
   ![Click the pop out button.](images/ClickPopOut.png)

85. A new tab should open. Your dashboard should look like the following image.
   ![Dashboard image.](images/Dashboard.png)

86. To interact with your simulated RaspberryPi, go back to your Node-RED tab and **select** the *Gauge Simulator* tab.
   ![Select the Gauge Simulator tab.](images/GaugeSimulator.png)

87. **Click** on the *Sensor Gauge Simulator* node.
   ![Click on the Sensor Gauge Simulator node.](images/GaugeNode.png)

88. On the right side of the browser, **click** on the *Info* tab.
   ![Click Info.](images/ClickInfo.png)

89. In the third paragraph under Information there is a hyperlink for the word *here*.  That hyperlink opens the simulator in a new tab. **Click** *here*.
   ![Click here.](images/ClickHere.png)

90. **Adjust** the temperature of the sensor in the simulator from 20 C.
   ![Adjust the sensor temperature.](images/Sensor.png)

91. Switch to the tab for your dashboard, notice the change to the sensor temperature and graph.
   ![Notice the sensor differences in the dashboard.](images/SensorDashboard.png)

92. Continue to play with your dashboard to interact with blockchain via API. You can try the following:

   - Type in a team name & click *Add Team Name*. You should see a successful message afterward.
     ![add team button](images/dashboard-add-team-button.png)
     ![deploy button](images/dashboard-status-team-added.png)

   - Change the thermostat by moving the slider next to *Thermostat Value*. Click *Change Thermostat*  to send the value to blockchain.'
     ![thermo slider](images/dashboard-thermo-slider.png)'
     ![change thermo button](images/dashboard-changethermo-button.png)

   - Type in a City and State to find outside temperatures. Click *Get Current Temps* to get the values.
     ![Enter a city and state. Select Get Current Temps.](images/OutsideTemps.png)

   - After you have outside temperatures, you can click *Get Recommendation* to find the ideal temperatures for the thermostat. Notice that this will adjust your thermostat in the dashboard.

   - ![recommendation result](images/dashboard-recommendation-result.png)

93. Congratulations! You've completed this exercise!



