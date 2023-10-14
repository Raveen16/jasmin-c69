*STEP 1* : Make a button/touchable opacity which will
trigger the QR code scanner and display the scanned
output as text.

*STEP 2* : Styling the button and its text

*STEP 3* : import components from respective libraries

*STEP 4* : define state object

*STEP 5* : write a function - getCameraPermission - which can request for camera permission 
& then call the function

*STEP 6* : what to do if,
hasCameraPermissions is 'null' or 'false'.

using ternary operator

*STEP 7* : return a BarCodeScanner component
when the button is clicked
write a function called handleBarCodeScanned()
which is called when the scan is completed


*Step1*
<TouchableOpacity style={[styles.button>
    <Text style={styles.buttonText}>Scan QR Code</Text>
</TouchableOpacity>



*Step2*
,
  button: {
    width: "43%",
    height: 55,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#F48D20",
    borderRadius: 15
  },
  buttonText: {
    fontSize: 24,
    color: "#FFFFFF"
  }

*Step3*
expo install expo-barcode-scanner

expo install expo-permissions

*Also step3*
import * as Permissions from "expo-permissions";
import { BarCodeScanner } from "expo-barcode-scanner";


*Step4*
Let's define following states in our application:
● domState : “ => This will tell if the app is in scanner mode or scanned mode.
● hasCameraPermissions: null => This will tell if the user has granted camera permission to the application.
● scanned: false => This will tell if scanning has been completed or not.
● scannedData: '' => This will hold the scanned data that we get after scanning.


 constructor(props) {
    super(props);
    this.state = {
      domState: "normal",
      hasCameraPermissions: null,
      scanned: false,
      scannedData: ""
    };
  }


*step5*

  getCameraPermissions = async domState => {
    const { status } = await Permissions.askAsync(Permissions.CAMERA);

    this.setState({
      /*status === "granted" is true when user has granted permission
          status === "granted" is false when user has not granted the permission
        */
      hasCameraPermissions: status === "granted",
      domState: domState,
      scanned: false
    });
  };


The Permission component which we imported has a predefined function called .askAsync() which can request various permissions.

Note: .askAsync() returns an object with a 'status' key containing the status of the permission granted by the user. 

If the user  grants permission, status changes to 'granted'

*Note: {status} automatically extracts the value from the object with key 'status'.






*calling the function*

   onPress={() => this.getCameraPermissions("scanner")}


*Step7*
We need to tell our application what to do if,
hasCameraPermissions is 'null' or 'false'.
Let's say that we want to display a text "Request for
camera permission" if hasCameraPermissions is false or
null.
If hasCameraPermissions is 'true', we will display
whatever text is inside the scannedData state. Currently
scannedData is an empty string.



*render*
    const { domState, hasCameraPermissions, scannedData, scanned } = this.state;


*return*
<Text style={styles.text}>
          {hasCameraPermissions ? scannedData : "Request for Camera Permission"}
</Text>




*step7*
handleBarCodeScanned = async ({ type, data }) => {
    this.setState({
      scannedData: data,
      domState: "normal",
      scanned: true
    });
  };



if (domState === "scanner") {
      return (
        <BarCodeScanner
          onBarCodeScanned={scanned ? undefined : this.handleBarCodeScanned}
          style={StyleSheet.absoluteFillObject}
        />
      );
    }