```
mushroom_seller/
├── lib/
│   ├── main.dart
│   ├── firebase_options.dart
│   ├── models/
│   │   └── sale.dart
│   ├── services/
│   │   ├── firestore_service.dart
│   │   ├── excel_service.dart
│   │   └── whatsapp_service.dart
│   ├── screens/
│   │   ├── home_screen.dart
│   │   ├── bill_preview_screen.dart
│   │   └── history_screen.dart
│   └── widgets/
│       └── app_widgets.dart
├── android/
│   └── app/
│       └── src/
│           └── main/
│               ├── AndroidManifest.xml
│               └── res/
│                   └── xml/
│                       └── file_paths.xml
└── pubspec.yaml
```

---

## 📄 File 1 — `pubspec.yaml` (root folder)

```yaml
name: mushroom_seller
description: Mushroom seller billing app
publish_to: 'none'
version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.24.2
  cloud_firestore: ^4.13.6
  excel: ^4.0.2
  path_provider: ^2.1.2
  url_launcher: ^6.2.4
  intl: ^0.19.0
  share_plus: ^7.2.2
  cupertino_icons: ^1.0.6

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^3.0.0

flutter:
  uses-material-design: true
```

---

## 📄 File 2 — `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'screens/home_screen.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(const MushroomSellerApp());
}

class MushroomSellerApp extends StatelessWidget {
  const MushroomSellerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Mushroom Seller',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF4E7C59),
        ),
        useMaterial3: true,
        inputDecorationTheme: InputDecorationTheme(
          filled: true,
          fillColor: const Color(0xFFF0F7F2),
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(16),
            borderSide:
                const BorderSide(color: Color(0xFF4E7C59), width: 1.5),
          ),
          enabledBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(16),
            borderSide:
                const BorderSide(color: Color(0xFFA8D5B5), width: 1.5),
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(16),
            borderSide:
                const BorderSide(color: Color(0xFF4E7C59), width: 2.5),
          ),
          labelStyle: const TextStyle(
            color: Color(0xFF4E7C59),
            fontWeight: FontWeight.w600,
          ),
          contentPadding:
              const EdgeInsets.symmetric(horizontal: 20, vertical: 18),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: const Color(0xFF4E7C59),
            foregroundColor: Colors.white,
            minimumSize: const Size(double.infinity, 60),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
            textStyle: const TextStyle(
                fontSize: 18, fontWeight: FontWeight.w700),
          ),
        ),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF4E7C59),
          foregroundColor: Colors.white,
          elevation: 0,
          centerTitle: true,
          titleTextStyle: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.w800,
            color: Colors.white,
          ),
        ),
      ),
      home: const HomeScreen(),
    );
  }
}
## 📄 File 3 — `lib/firebase_options.dart`

> ⚠️ This is a placeholder. You MUST replace it after running `flutterfire configure` in Codespaces.

```dart
import 'package:firebase_core/firebase_core.dart' show FirebaseOptions;
import 'package:flutter/foundation.dart'
    show defaultTargetPlatform, kIsWeb, TargetPlatform;

class DefaultFirebaseOptions {
  static FirebaseOptions get currentPlatform {
    if (kIsWeb) return web;
    switch (defaultTargetPlatform) {
      case TargetPlatform.android:
        return android;
      case TargetPlatform.iOS:
        return ios;
      default:
        throw UnsupportedError('Unsupported platform');
    }
  }

  static const FirebaseOptions web = FirebaseOptions(
    apiKey: 'YOUR_WEB_API_KEY',
    appId: 'YOUR_WEB_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
    authDomain: 'YOUR_PROJECT_ID.firebaseapp.com',
    storageBucket: 'YOUR_PROJECT_ID.appspot.com',
  );

  static const FirebaseOptions android = FirebaseOptions(
    apiKey: 'YOUR_ANDROID_API_KEY',
    appId: 'YOUR_ANDROID_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
    storageBucket: 'YOUR_PROJECT_ID.appspot.com',
  );

  static const FirebaseOptions ios = FirebaseOptions(
    apiKey: 'YOUR_IOS_API_KEY',
    appId: 'YOUR_IOS_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
    storageBucket: 'YOUR_PROJECT_ID.appspot.com',
    iosBundleId: 'com.example.mushroomSeller',
  );
}
```

---

## 📄 File 4 — `lib/models/sale.dart`

```dart
class Sale {
  final String? id;
  final String customerName;
  final String mushroomType;
  final double quantity;
  final double ratePerKg;
  final double totalPrice;
  final DateTime date;

  Sale({
    this.id,
    required this.customerName,
    required this.mushroomType,
    required this.quantity,
    required this.ratePerKg,
    required this.totalPrice,
    required this.date,
  });

  Map<String, dynamic> toMap() {
    return {
      'customerName': customerName,
      'mushroomType': mushroomType,
      'quantity': quantity,
      'ratePerKg': ratePerKg,
      'totalPrice': totalPrice,
      'date': date.toIso8601String(),
    };
  }

  factory Sale.fromMap(String id, Map<String, dynamic> map) {
    return Sale(
      id: id,
      customerName: map['customerName'] ?? '',
      mushroomType: map['mushroomType'] ?? '',
      quantity: (map['quantity'] as num).toDouble(),
      ratePerKg: (map['ratePerKg'] as num).toDouble(),
      totalPrice: (map['totalPrice'] as num).toDouble(),
      date: DateTime.parse(map['date']),
    );
  }
}
```

---

## 📄 File 5 — `lib/services/firestore_service.dart`

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/sale.dart';

class FirestoreService {
  static final FirestoreService _instance = FirestoreService._internal();
  factory FirestoreService() => _instance;
  FirestoreService._internal();

  final FirebaseFirestore _db = FirebaseFirestore.instance;
  static const String _collection = 'sales';

  Future<String> addSale(Sale sale) async {
    try {
      final docRef = await _db.collection(_collection).add(sale.toMap());
      return docRef.id;
    } catch (e) {
      throw Exception('Failed to save sale: $e');
    }
  }

  Future<List<Sale>> getAllSales() async {
    try {
      final snapshot = await _db
          .collection(_collection)
          .orderBy('date', descending: true)
          .get();
      return snapshot.docs
          .map((doc) => Sale.fromMap(doc.id, doc.data()))
          .toList();
    } catch (e) {
      throw Exception('Failed to load sales: $e');
    }
  }

  Stream<List<Sale>> salesStream() {
    return _db
        .collection(_collection)
        .orderBy('date', descending: true)
        .snapshots()
        .map((snapshot) => snapshot.docs
            .map((doc) => Sale.fromMap(doc.id, doc.data()))
            .toList());
  }

  Future<void> deleteSale(String id) async {
    try {
      await _db.collection(_collection).doc(id).delete();
    } catch (e) {
      throw Exception('Failed to delete sale: $e');
    }
  }
}
```

---

## 📄 File 6 — `lib/services/excel_service.dart`

```dart
import 'dart:io';
import 'package:excel/excel.dart';
import 'package:path_provider/path_provider.dart';
import 'package:share_plus/share_plus.dart';
import 'package:intl/intl.dart';
import '../models/sale.dart';

class ExcelService {
  static Future<void> exportSales(List<Sale> sales) async {
    if (sales.isEmpty) throw Exception('No sales data to export.');

    final excel = Excel.createExcel();
    excel.delete('Sheet1');
    final Sheet sheet = excel['Sales Report'];

    final headers = [
      'Date', 'Customer Name', 'Mushroom Type',
      'Quantity (kg)', 'Rate/kg (₹)', 'Total (₹)',
    ];

    final headerStyle = CellStyle(
      bold: true,
      backgroundColorHex: ExcelColor.fromHexString('#4E7C59'),
      fontColorHex: ExcelColor.fromHexString('#FFFFFF'),
    );

    for (int col = 0; col < headers.length; col++) {
      final cell = sheet.cell(
        CellIndex.indexByColumnRow(columnIndex: col, rowIndex: 0),
      );
      cell.value = TextCellValue(headers[col]);
      cell.cellStyle = headerStyle;
    }

    final dateFormat = DateFormat('dd/MM/yyyy HH:mm');
    for (int i = 0; i < sales.length; i++) {
      final sale = sales[i];
      final rowData = [
        dateFormat.format(sale.date),
        sale.customerName,
        sale.mushroomType,
        sale.quantity.toStringAsFixed(2),
        sale.ratePerKg.toStringAsFixed(2),
        sale.totalPrice.toStringAsFixed(2),
      ];
      for (int col = 0; col < rowData.length; col++) {
        sheet
            .cell(CellIndex.indexByColumnRow(columnIndex: col, rowIndex: i + 1))
            .value = TextCellValue(rowData[col]);
      }
    }

    final totalSales = sales.fold<double>(0, (sum, s) => sum + s.totalPrice);
    final totalRow = sales.length + 2;
    sheet
        .cell(CellIndex.indexByColumnRow(columnIndex: 0, rowIndex: totalRow))
        .value = TextCellValue('TOTAL');
    sheet
        .cell(CellIndex.indexByColumnRow(columnIndex: 5, rowIndex: totalRow))
        .value = TextCellValue('₹${totalSales.toStringAsFixed(2)}');

    final bytes = excel.encode();
    if (bytes == null) throw Exception('Failed to encode Excel file.');

    final directory = await getTemporaryDirectory();
    final fileName =
        'MushroomSales_${DateFormat('yyyyMMdd_HHmm').format(DateTime.now())}.xlsx';
    final file = File('${directory.path}/$fileName');
    await file.writeAsBytes(bytes);

    await Share.shareXFiles(
      [XFile(file.path)],
      subject: 'Mushroom Sales Report',
    );
  }
}
```

---

## 📄 File 7 — `lib/services/whatsapp_service.dart`

```dart
import 'package:url_launcher/url_launcher.dart';
import 'package:intl/intl.dart';
import '../models/sale.dart';

class WhatsAppService {
  static Future<void> sendBill({
    required Sale sale,
    String phoneNumber = '',
  }) async {
    final dateStr = DateFormat('dd MMM yyyy, hh:mm a').format(sale.date);

    final message = '''
🍄 *MUSHROOM BILL* 🍄
━━━━━━━━━━━━━━━━━━━━━
📅 Date: $dateStr

👤 Customer: ${sale.customerName}
🌿 Mushroom Type: ${sale.mushroomType}
⚖️ Quantity: ${sale.quantity.toStringAsFixed(2)} kg
💰 Rate: ₹${sale.ratePerKg.toStringAsFixed(2)} / kg
━━━━━━━━━━━━━━━━━━━━━
💵 *TOTAL: ₹${sale.totalPrice.toStringAsFixed(2)}*
━━━━━━━━━━━━━━━━━━━━━
Thank you for your purchase! 🙏
''';

    final encodedMessage = Uri.encodeComponent(message);
    final url = phoneNumber.isEmpty
        ? 'https://wa.me/?text=$encodedMessage'
        : 'https://wa.me/$phoneNumber?text=$encodedMessage';

    final uri = Uri.parse(url);
    if (await canLaunchUrl(uri)) {
      await launchUrl(uri, mode: LaunchMode.externalApplication);
    } else {
      throw Exception('WhatsApp could not be opened.');
    }
  }
}
```

---

## 📄 File 8 — `lib/widgets/app_widgets.dart`

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class BigTextField extends StatelessWidget {
  final String label;
  final String hint;
  final IconData icon;
  final TextEditingController controller;
  final TextInputType keyboardType;
  final List<TextInputFormatter>? inputFormatters;
  final String? Function(String?)? validator;
  final void Function(String)? onChanged;

  const BigTextField({
    super.key,
    required this.label,
    required this.hint,
    required this.icon,
    required this.controller,
    this.keyboardType = TextInputType.text,
    this.inputFormatters,
    this.validator,
    this.onChanged,
  });

  @override
  Widget build(BuildContext context) {
    return TextFormField(
      controller: controller,
      keyboardType: keyboardType,
      inputFormatters: inputFormatters,
      validator: validator,
      onChanged: onChanged,
      style: const TextStyle(
        fontSize: 17,
        fontWeight: FontWeight.w600,
        color: Color(0xFF1B3A28),
      ),
      decoration: InputDecoration(
        labelText: label,
        hintText: hint,
        hintStyle: TextStyle(color: Colors.grey.shade400),
        prefixIcon: Icon(icon, color: const Color(0xFF4E7C59), size: 24),
      ),
    );
  }
}

class SectionHeading extends StatelessWidget {
  final String text;
  const SectionHeading(this.text, {super.key});

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.only(bottom: 12),
      child: Text(
        text,
        style: const TextStyle(
          fontSize: 13,
          fontWeight: FontWeight.w700,
          color: Color(0xFF4E7C59),
          letterSpacing: 1.2,
        ),
      ),
    );
  }
}

class ActionButton extends StatelessWidget {
  final String label;
  final IconData icon;
  final Color color;
  final VoidCallback? onTap;
  final bool loading;

  const ActionButton({
    super.key,
    required this.label,
    required this.icon,
    required this.color,
    this.onTap,
    this.loading = false,
  });

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      height: 60,
      width: double.infinity,
      child: ElevatedButton.icon(
        style: ElevatedButton.styleFrom(
          backgroundColor: color,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(16),
          ),
        ),
        onPressed: loading ? null : onTap,
        icon: loading
            ? const SizedBox(
                width: 22,
                height: 22,
                child: CircularProgressIndicator(
                    color: Colors.white, strokeWidth: 2.5),
              )
            : Icon(icon, size: 24),
        label: Text(
          label,
          style: const TextStyle(fontSize: 17, fontWeight: FontWeight.w700),
        ),
      ),
    );
  }
}

void showSuccess(BuildContext context, String message) {
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(
      content: Row(children: [
        const Icon(Icons.check_circle, color: Colors.white),
        const SizedBox(width: 10),
        Expanded(child: Text(message)),
      ]),
      backgroundColor: const Color(0xFF4E7C59),
      behavior: SnackBarBehavior.floating,
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      margin: const EdgeInsets.all(16),
    ),
  );
}

void showError(BuildContext context, String message) {
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(
      content: Row(children: [
        const Icon(Icons.error_outline, color: Colors.white),
        const SizedBox(width: 10),
        Expanded(child: Text(message)),
      ]),
      backgroundColor: Colors.redAccent,
      behavior: SnackBarBehavior.floating,
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      margin: const EdgeInsets.all(16),
    ),
  );
}
```

---

## 📄 File 9 — `lib/screens/home_screen.dart`

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:intl/intl.dart';
import '../models/sale.dart';
import '../services/firestore_service.dart';
import '../services/whatsapp_service.dart';
import '../widgets/app_widgets.dart';
import 'bill_preview_screen.dart';
import 'history_screen.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  final _formKey = GlobalKey<FormState>();
  final _nameCtrl = TextEditingController();
  final _mushroomCtrl = TextEditingController();
  final _quantityCtrl = TextEditingController();
  final _rateCtrl = TextEditingController();

  double _totalPrice = 0.0;
  bool _isSaving = false;
  Sale? _lastSavedSale;

  final List<String> _mushroomTypes = [
    'Button Mushroom', 'Oyster Mushroom', 'Shiitake Mushroom',
    'Portobello Mushroom', 'Cremini Mushroom', 'Enoki Mushroom',
  ];

  void _calculateTotal() {
    final qty = double.tryParse(_quantityCtrl.text) ?? 0.0;
    final rate = double.tryParse(_rateCtrl.text) ?? 0.0;
    setState(() => _totalPrice = qty * rate);
  }

  Future<Sale?> _saveSale() async {
    if (!_formKey.currentState!.validate()) return null;
    setState(() => _isSaving = true);
    try {
      final sale = Sale(
        customerName: _nameCtrl.text.trim(),
        mushroomType: _mushroomCtrl.text.trim(),
        quantity: double.parse(_quantityCtrl.text),
        ratePerKg: double.parse(_rateCtrl.text),
        totalPrice: _totalPrice,
        date: DateTime.now(),
      );
      final id = await FirestoreService().addSale(sale);
      return Sale(
        id: id,
        customerName: sale.customerName,
        mushroomType: sale.mushroomType,
        quantity: sale.quantity,
        ratePerKg: sale.ratePerKg,
        totalPrice: sale.totalPrice,
        date: sale.date,
      );
    } catch (e) {
      if (mounted) showError(context, 'Error saving: $e');
      return null;
    } finally {
      if (mounted) setState(() => _isSaving = false);
    }
  }

  Future<void> _onGenerateBill() async {
    final sale = await _saveSale();
    if (sale == null || !mounted) return;
    setState(() => _lastSavedSale = sale);
    showSuccess(context, 'Bill saved!');
    Navigator.push(context,
        MaterialPageRoute(builder: (_) => BillPreviewScreen(sale: sale)));
  }

  Future<void> _onSendWhatsApp() async {
    Sale? sale = _lastSavedSale;
    if (sale == null) {
      sale = await _saveSale();
      if (sale == null) return;
      setState(() => _lastSavedSale = sale);
    }
    try {
      await WhatsAppService.sendBill(sale: sale);
    } catch (e) {
      if (mounted) showError(context, e.toString());
    }
  }

  void _clearForm() {
    _nameCtrl.clear();
    _mushroomCtrl.clear();
    _quantityCtrl.clear();
    _rateCtrl.clear();
    setState(() { _totalPrice = 0.0; _lastSavedSale = null; });
  }

  @override
  void dispose() {
    _nameCtrl.dispose(); _mushroomCtrl.dispose();
    _quantityCtrl.dispose(); _rateCtrl.dispose();
    super.dispose();
  }

  static final _decimalFormatter =
      FilteringTextInputFormatter.allow(RegExp(r'^\d+\.?\d{0,2}'));

  @override
  Widget build(BuildContext context) {
    final currencyFmt =
        NumberFormat.currency(symbol: '₹', decimalDigits: 2);

    return Scaffold(
      backgroundColor: const Color(0xFFF5FBF7),
      appBar: AppBar(
        title: const Text('🍄 Mushroom Seller'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history_rounded),
            onPressed: () => Navigator.push(context,
                MaterialPageRoute(builder: (_) => const HistoryScreen())),
          ),
        ],
      ),
      body: SafeArea(
        child: SingleChildScrol
