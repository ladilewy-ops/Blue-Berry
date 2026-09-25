1. Your Client App Requirements
The first version will have:
1.	Client registration/login
o	Name
o	Phone number
o	Password or phone authentication
o	Basic profile
2.	Request my bike
o	Your bike is the default option.
o	Customer doesn't need to select from multiple bikes initially.
o	A simple Request Bike button starts the process.
3.	Pickup location
o	Detect customer's current GPS location.
o	Allow them to change/search for the pickup point.
4.	Drop-off location
o	Search for destination.
o	Display it on a map.
5.	Fare calculation
o	Show the customer the estimated fare before requesting the ride.
o	We can define your pricing formula later, for example:
Plain Text
Fare = Base Fare + (Distance × Price per km)
Show more lines
6.	Track your bike
o	Map showing the customer's pickup point and your bike/driver location.
o	Once the driver accepts the request, the customer can follow the bike's location.
7.	Call Driver
o	A Call Driver button.
o	For the first version, it can open the phone dialer with your business/driver number.
8.	Cancel request
o	Customer can cancel an active request.
o	Record the cancelled ride in the database.
9.	M-PESA payment
o	The proper integration point is Safaricom's Daraja platform. Safaricom describes Daraja 3.0 as the platform connecting web/mobile apps to M-PESA APIs, and its M-PESA Express API can initiate a Buy Goods or Pay Bill payment prompt from the customer's account. [developer....icom.co.ke], [developer....icom.co.ke]
o	The app can therefore have a Pay with M-PESA flow connected to your business payment setup.
10.	Ride history
•	Date
•	Pickup
•	Destination
•	Fare
•	Ride status
•	Payment status
________________________________________
2. Tools I Recommend
For your particular project, I recommend this stack:
Mobile application: Flutter
Flutter + Dart
This will be the actual client app.
One major advantage is that we can build Android and iOS from the same Flutter project. Google's Maps documentation provides a Flutter Maps package targeting Android, iOS and Web. [developers...google.com]
Since you're starting out, I'd concentrate on Android first, then make the iPhone version later.
________________________________________
Development environment: Visual Studio Code
I recommend:
VS Code + Flutter SDK + Dart
We'll write the application here.
You should also install Android Studio primarily for its Android SDK and emulator.
So:
Plain Text
VS Code
↓
Flutter / Dart
↓
Android emulator / Android phone
Show more lines
________________________________________
3. Backend: Firebase
For the first version, I recommend Firebase.
Firebase officially provides Flutter plugins for Authentication, Firestore, Cloud Functions, Cloud Messaging and other services. [firebase.google.com]
We can use:
Firebase Authentication
For:
Plain Text
Register
Login
Logout
User accounts
Show more lines
Cloud Firestore
Our database.
For example:
Plain Text
users
rides
drivers
bike
payments
Show more lines
A ride could eventually look roughly like:
Plain Text
Ride
 
rideId
clientId
driverId
pickupLocation
dropoffLocation
pickupLatitude
pickupLongitude
dropoffLatitude
dropoffLongitude
distance
fare
status
paymentStatus
createdAt
Show more lines
This becomes important when we develop your driver app.
Both apps will access the same ride system:
Plain Text
CLIENT APP
|
| Creates ride request
↓
FIREBASE / BACKEND
|
| New ride
↓
DRIVER APP
Show more lines
Firebase's Firestore supports realtime listeners for keeping data synchronized across client apps, which fits this architecture well. [firebase.google.com]
________________________________________
4. Maps and GPS: Google Maps
I recommend Google Maps Platform.
The official Flutter package provides a Google Maps widget for Flutter. [pub.dev]
We'll use the mapping/location services for:
Plain Text
Customer current location
↓
Pickup
 
Pickup
↓
Route
↓
Destination
 
Distance
↓
Fare calculation
Show more lines
And eventually:
Plain Text
🏍 Your bike
↓
Live location
↓
📍 Customer
Show more lines
________________________________________
5. M-PESA: Safaricom Daraja
For payments:
Safaricom Daraja API
Safaricom's current developer portal provides a sandbox where developers can create and test apps before integrating M-PESA. [developer....icom.co.ke]
Ultimately, the customer experience could be:
Plain Text
Ride Fare
 
KSh 250
 
[ PAY WITH M-PESA ]
 
↓
 
Customer receives M-PESA prompt
 
↓
 
Customer enters M-PESA PIN
 
↓
 
Payment confirmation
 
↓
 
Ride marked PAID
``
Show more lines
One important detail: because your requirement is that the money goes directly to your M-PESA, we'll need to determine the appropriate merchant/payment setup when we reach the payment phase rather than assuming that an ordinary personal phone number works exactly like a business Till/PayBill integration.
________________________________________
6. Calling You
This part can remain extremely simple.
The app would have:
📞 Call Driver
Pressing it opens the customer's phone dialer with your number.
We don't need WhatsApp, VoIP, or an expensive communications service for version 1.
________________________________________
7. Suggested Client Screens
I'd make the initial app about 8 main screens:
Screen 1: Welcome
Plain Text
🏍
 
My Bike
 
Fast. Simple. Reliable.
 
[ LOGIN ]
 
[ CREATE ACCOUNT ]
Show more lines
Screen 2: Registration
Plain Text
Create Account
 
Full Name
Phone Number
Password
Confirm Password
 
[ CREATE ACCOUNT ]
Show more lines
Screen 3: Home
Map at the top and:
Plain Text
Where are you going?
 
Pickup
📍 Current Location
 
Destination
📍 Enter destination
 
 
YOUR BIKE
 
🏍
 
[ REQUEST BIKE ]
Show more lines
________________________________________
Screen 4: Ride quotation
Plain Text
Pickup
Karingani
 
↓
 
Destination
...
 
Distance: 4.8 km
 
Estimated Fare:
KSh 200
 
[ CONFIRM REQUEST ]
 
[ CANCEL ]
``
Show more lines
________________________________________
Screen 5: Looking for bike
Plain Text
Requesting your bike...
 
🏍
 
Pickup: ...
Destination: ...
Fare: KSh 200
 
[ CANCEL REQUEST ]
``
Show more lines
Then once you accept from the future driver app:
Plain Text
Driver is coming
 
🏍
MAP
 
5 minutes away
 
[ CALL DRIVER ]
 
[ CANCEL RIDE ]
Show more lines
________________________________________
Screen 6: Ride tracking
The customer gets essentially a full-screen map:
Plain Text
🏍
\
\
\
📍 Customer
``
Show more lines
Your driver app will continuously update the bike location and the customer app will display it.
________________________________________
Screen 7: Payment
Plain Text
Ride Complete
 
Fare
KSh 250
 
Payment Method
 
○ M-PESA
 
[ PAY KSh 250 ]
 
Payment status:
PAID ✓
Show more lines
The M-PESA integration would happen through Daraja. [developer....icom.co.ke], [developer....icom.co.ke]
________________________________________
Screen 8: Ride History
Plain Text
Ride History
 
Today
Karingani → Destination
KSh 250
Completed ✓
 
22 Sep
Pickup → Destination
KSh 180
Completed ✓
 
20 Sep
Pickup → Destination
Cancelled
Show more lines
________________________________________
8. How the Complete System Will Eventually Work
This is the important part.
We're not really building two completely independent apps.
We're building:
Plain Text
┌─────────────────┐
│ CLIENT APP │
│ Flutter │
└────────┬────────┘
│
Request Ride
│
▼
┌──────────────┐
│ BACKEND │
│ Firebase │
└───────┬──────┘
│
New Request
│
▼
┌─────────────────┐
│ DRIVER APP │
│ Flutter │
└─────────────────┘
│
Accept
│
▼
Client notified
``
Show more lines
The client app comes first, but we'll structure it with the eventual driver app in mind.
________________________________________
9. Your Main Development Toolkit
So your starting toolbox is:
Plain Text
Flutter
Dart
VS Code
Android Studio / Android SDK
 
Firebase
├── Authentication
├── Firestore
├── Cloud Functions
└── Cloud Messaging
 
Google Maps Platform
├── Maps
├── Locations
└── Routes/distance functions as needed
 
Safaricom Daraja
└── M-PESA payments
 
Git + GitHub
└── Source-code/version control
 
Show more lines
Firebase has current official setup instructions specifically for connecting Flutter apps, including the FlutterFire configuration workflow. [firebase.google.com]
Recommended development order
Don't start with M-PESA or live tracking immediately. I'd build your first working version in this order:
Phase 1: Flutter project + design/navigation
Phase 2: Registration/login
Phase 3: Google Map + current location
Phase 4: Pickup + destination
Phase 5: Distance + fare calculation
Phase 6: Request Bike + Firebase ride record
Phase 7: Cancellation + ride history
Phase 8: Driver calling
Phase 9: Live bike tracking
Phase 10: M-PESA
Phase 11: Build the corresponding driver app
you
