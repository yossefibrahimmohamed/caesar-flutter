import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: CaesarCipherApp(),
  ));
}
class CaesarCipherApp extends StatefulWidget {
  @override
  _CaesarCipherAppState createState() => _CaesarCipherAppState();
}

class _CaesarCipherAppState extends State<CaesarCipherApp> {
  TextEditingController _controller = TextEditingController();
  TextEditingController _controller2 = TextEditingController();
  int _shift = 3;
  String _resultencryption = '';
  String _resultdecryption = '';
  void _encrypt() { // c = p+k mod 26
    String plainText = _controller.text;
    String cipherText = '';
    for (int i = 0; i < plainText.length; i++) {
      String char = plainText[i];
      if (char.contains(RegExp(r'[A-Za-z]'))) {
        // Convert the character to its ASCII code
        int asciiCode = char.codeUnitAt(0);
        // Determine if the character is uppercase or lowercase
        if (char == char.toUpperCase()) {
          // Apply the shift to the ASCII code of the character, wrapping around if necessary
          int shiftedCode = (asciiCode - 65 + _shift) % 26 + 65;
          // Convert the shifted ASCII code back to a character
          char = String.fromCharCode(shiftedCode);
        } else {
          int shiftedCode = (asciiCode - 97 + _shift) % 26 + 97;
          char = String.fromCharCode(shiftedCode);
        }
      }
      // Append the encrypted character to the cipher text
      cipherText += char;
    }
    setState(() {
      _resultencryption = cipherText;
    });
  }
  void _Decrypt() { // p = c+k mod 26
    String cipherText = _controller2.text;
    String plainText = '';
    for (int i = 0; i < cipherText.length; i++) {
      String char = cipherText[i];
      if (char.contains(RegExp(r'[A-Za-z]'))) {
        int asciiCode = char.codeUnitAt(0);
        if (char == char.toUpperCase()) {
          int shiftedCode = (asciiCode - 65 - _shift) % 26 + 65;
          char = String.fromCharCode(shiftedCode);
        } else {
          int shiftedCode = (asciiCode - 97 - _shift) % 26 + 97;
          char = String.fromCharCode(shiftedCode);
        }
      }
      plainText += char;
    }
    setState(() {
      _resultdecryption  = plainText;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Caesar Cipher'),
        backgroundColor: Colors.red,
      ),
      body: Container(
        padding: EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(

              controller: _controller,
              decoration: InputDecoration(
                hintText: 'Enter a message to encrypt',
              ),
            ),
            SizedBox(height: 20,),
            TextField(
              controller: _controller2,
              decoration: InputDecoration(
                hintText: 'Enter a message to Decrypt',
              ),
            ),
            SizedBox(height: 16.0),
            Row(
              children: [
                Text('key: '),
                SizedBox(width: 16.0),
                DropdownButton<int>(
                  value: _shift,
                  items: List.generate(26, (i) => i + 1)
                      .map((i) => DropdownMenuItem(
                    value: i,
                    child: Text(i.toString()),
                  ))
                      .toList(),
                  onChanged: (value) {
                    setState(() {
                      _shift = value!;
                    });
                  },
                ),
              ],
            ),
            SizedBox(height: 16.0),
            Row(
              children: [
                ElevatedButton(
                  onPressed: _encrypt,
                  child: Text('Encrypt'),

                ),
                SizedBox(width: 150,),
                ElevatedButton(
                  onPressed: _Decrypt,
                  child: Text('Decrypt'),

                ),
              ],
            ),
            Column(
              mainAxisAlignment: MainAxisAlignment.start,
              children: [
              SizedBox(height: 10.0),
              Text('Encrypt'),
              TextField(
                enabled: false,
                decoration: InputDecoration(
                  hintText: _resultencryption,
                ),
              ),
            SizedBox(height: 8.0),
            Text('Decrypt'),
                SingleChildScrollView(child:Column(
                  children: [
                  TextField(
                    enabled: false,
                    decoration: InputDecoration(
                      hintText: _resultdecryption,
                      ),
                    ),
                  ],
                )
              ),
            ],
          )
        ],
      ),
    ),
  );
}
}


