# Flutter-Rest-Api-Call-Sample
This is a demo app to learn how to call rest api in flutter app. I use this [fake rest api](https://jsonplaceholder.typicode.com/users) for this sample.


## Add required library under dependencies section in your pubspec file
```dart
http: ^0.12.0+4
```

## Create Json Response Parser classes
```dart
class User{
  int id;
  String name,email,phone,website;
  Address address;
  Company company;

  User({this.id, this.name, this.email, this.phone, this.website, this.address,
      this.company});

  factory User.fromJSON(Map<String, dynamic> parsedJson){
    return User(
      id: parsedJson['id'],
      name: parsedJson['name'],
      email: parsedJson['email'],
      phone: parsedJson['phone'],
      website: parsedJson['website'],
      address: Address.fromJSON(parsedJson['address']),
      company: Company.fromJSON(parsedJson['company']),
    );
  }
}


class Address{
  String street, suite, city, zipcode;

  Address({this.street, this.suite, this.city, this.zipcode});

  factory Address.fromJSON(Map<String, dynamic> parsedJson){
    return Address(
      street: parsedJson['street'],
      suite: parsedJson['suite'],
      city: parsedJson['city'],
      zipcode: parsedJson['zipcode'],
    );
  }
}


class Company{
  String name, catchPhrase, bs;

  Company({this.name, this.catchPhrase, this.bs});

  factory Company.fromJSON(Map<String, dynamic> parsedJson){
    return Company(
        name: parsedJson['name'],
        catchPhrase: parsedJson['catchPhrase'],
        bs: parsedJson['bs']
    );
  }
}
```

## Create async method for api call
```dart
import 'package:http/http.dart' as http;
Future<List<User>> fetchUsers() async {
    final response =
        await http.get('https://jsonplaceholder.typicode.com/users');
    if (response.statusCode == 200) {
      var getUsersData = json.decode(response.body) as List;
      var listUsers = getUsersData.map((i) => User.fromJSON(i)).toList();
      return listUsers;
    } else {
      throw Exception('Failed to load users');
    }
  }
```

## Call created async method
```dart
Future<List<User>> listUsers;

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    listUsers = fetchUsers();
  }
```

## Display data
```dart
@override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
        body: FutureBuilder<List<User>>(
      future: listUsers,
      builder: (context, snapshot) {
        if (snapshot.hasData) {
             
        } else if (snapshot.hasError) {
          return Center(
            child: Text("${snapshot.error}"),
          );
        }
        return Center(
          child: CircularProgressIndicator(
            backgroundColor: Colors.cyanAccent,
          ),
        );
      },
    ));
  }
```
