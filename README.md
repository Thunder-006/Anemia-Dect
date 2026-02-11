import 'package:flutter/material.dart';
import 'dart:io';
import 'package:image_picker/image_picker.dart';
import 'package:image/image.dart' as img;

void main() {
  runApp(AnemiaApp());
}

class AnemiaApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
        primaryColor: Color(0xFF0D47A1), // Medical blue
        colorScheme: ColorScheme.fromSeed(
          seedColor: Color(0xFF0D47A1),
          secondary: Color(0xFF00897B), // Teal
        ),
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: StartScreen(),
    );
  }
}

// ---------------- START SCREEN (UPGRADED) ----------------
class StartScreen extends StatelessWidget {
  void _showImpactDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Row(
          children: [
            Icon(Icons.lightbulb, color: Colors.orange),
            SizedBox(width: 10),
            Text("Why This App Matters?"),
          ],
        ),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildImpactPoint("India has >50% anemia prevalence in women"),
            _buildImpactPoint("Many avoid blood tests due to cost & fear"),
            _buildImpactPoint("Early symptom screening saves time & lives"),
            SizedBox(height: 15),
            Container(
              padding: EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: Colors.blue[50],
                borderRadius: BorderRadius.circular(8),
              ),
              child: Text(
                "This app bridges the gap between ignorance and diagnosis using AI-assisted early screening.",
                style: TextStyle(
                  fontStyle: FontStyle.italic,
                  color: Colors.blue[900],
                  fontWeight: FontWeight.w500,
                ),
              ),
            ),
          ],
        ),
        actions: [
          TextButton(
            child: Text("Got it!"),
            onPressed: () => Navigator.pop(context),
          ),
        ],
      ),
    );
  }

  Widget _buildImpactPoint(String text) {
    return Padding(
      padding: EdgeInsets.only(bottom: 8),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text("• ", style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
          Expanded(child: Text(text, style: TextStyle(fontSize: 14))),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      body: SafeArea(
        child: Center(
          child: Padding(
            padding: EdgeInsets.all(24),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                // Medical Icon
                Container(
                  padding: EdgeInsets.all(20),
                  decoration: BoxDecoration(
                    color: Color(0xFF0D47A1).withOpacity(0.1),
                    shape: BoxShape.circle,
                  ),
                  child: Icon(
                    Icons.local_hospital,
                    size: 80,
                    color: Color(0xFF0D47A1),
                  ),
                ),
                SizedBox(height: 30),
                
                // Title
                Text(
                  "🩺 AI-Powered Anemia",
                  style: TextStyle(
                    fontSize: 28,
                    fontWeight: FontWeight.bold,
                    color: Color(0xFF0D47A1),
                  ),
                  textAlign: TextAlign.center,
                ),
                Text(
                  "Risk Assessment",
                  style: TextStyle(
                    fontSize: 28,
                    fontWeight: FontWeight.bold,
                    color: Color(0xFF0D47A1),
                  ),
                  textAlign: TextAlign.center,
                ),
                SizedBox(height: 15),
                
                // Subtitle
                Text(
                  "Early screening using symptoms, hemoglobin data,\nand visual pallor analysis",
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 14,
                    color: Colors.grey[700],
                    height: 1.5,
                  ),
                ),
                SizedBox(height: 40),
                
                // Start Button
                ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Color(0xFF0D47A1),
                    padding: EdgeInsets.symmetric(horizontal: 50, vertical: 18),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    elevation: 3,
                  ),
                  child: Text(
                    "Start Screening",
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.w600, color: Colors.white),
                  ),
                  onPressed: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(builder: (context) => DecisionScreen()),
                    );
                  },
                ),
                SizedBox(height: 25),
                
                // Disclaimer
                Container(
                  padding: EdgeInsets.all(14),
                  decoration: BoxDecoration(
                    color: Colors.amber[50],
                    borderRadius: BorderRadius.circular(10),
                    border: Border.all(color: Colors.amber[300]!, width: 1.5),
                  ),
                  child: Row(
                    children: [
                      Icon(Icons.warning_amber, color: Colors.amber[900], size: 24),
                      SizedBox(width: 12),
                      Expanded(
                        child: Text(
                          "This tool is for early awareness,\nnot a medical diagnosis",
                          style: TextStyle(
                            fontSize: 13,
                            color: Colors.amber[900],
                            fontWeight: FontWeight.w500,
                            height: 1.4,
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
                SizedBox(height: 20),
                
                // Why This Matters Button
                TextButton.icon(
                  icon: Icon(Icons.lightbulb_outline, size: 20),
                  label: Text("Why This App Matters?"),
                  onPressed: () => _showImpactDialog(context),
                  style: TextButton.styleFrom(
                    foregroundColor: Color(0xFF0D47A1),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

// ---------------- DECISION SCREEN WITH STEP INDICATOR ----------------
class DecisionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Step 1: Basic Details", style: TextStyle(color: Colors.white)),
        centerTitle: true,
        backgroundColor: Color(0xFF0D47A1),
        iconTheme: IconThemeData(color: Colors.white),
      ),
      body: Center(
        child: Padding(
          padding: EdgeInsets.all(24),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.bloodtype, size: 70, color: Color(0xFFD32F2F)),
              SizedBox(height: 30),
              Text(
                "Do you have Hemoglobin (Hb) data?",
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.w600,
                  color: Color(0xFF0D47A1),
                ),
                textAlign: TextAlign.center,
              ),
              SizedBox(height: 12),
              Text(
                "Choose your assessment path",
                style: TextStyle(fontSize: 14, color: Colors.grey[600]),
              ),
              SizedBox(height: 50),
              
              // Yes Button
              Container(
                width: double.infinity,
                child: ElevatedButton.icon(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Color(0xFF00897B),
                    padding: EdgeInsets.symmetric(vertical: 18),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    elevation: 2,
                  ),
                  icon: Icon(Icons.check_circle, color: Colors.white, size: 24),
                  label: Text(
                    "Yes, I have Hb data",
                    style: TextStyle(fontSize: 17, fontWeight: FontWeight.w600, color: Colors.white),
                  ),
                  onPressed: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(builder: (context) => WithHbScreen()),
                    );
                  },
                ),
              ),
              SizedBox(height: 18),
              
              // No Button
              Container(
                width: double.infinity,
                child: OutlinedButton.icon(
                  style: OutlinedButton.styleFrom(
                    padding: EdgeInsets.symmetric(vertical: 18),
                    side: BorderSide(color: Color(0xFFFF6F00), width: 2),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                  ),
                  icon: Icon(Icons.assignment, color: Color(0xFFFF6F00), size: 24),
                  label: Text(
                    "No, symptom-based only",
                    style: TextStyle(fontSize: 17, fontWeight: FontWeight.w600, color: Color(0xFFFF6F00)),
                  ),
                  onPressed: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(builder: (context) => WithoutHbScreen()),
                    );
                  },
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// ================= PATH 1: WITH HB DATA (UPGRADED WITH PREGNANCY MONTH) =================
class WithHbScreen extends StatefulWidget {
  @override
  _WithHbScreenState createState() => _WithHbScreenState();
}

class _WithHbScreenState extends State<WithHbScreen> {
  final TextEditingController hbController = TextEditingController();
  final TextEditingController ageController = TextEditingController();
  final ImagePicker _picker = ImagePicker();
  
  File? _uploadedImage;
  bool _imageAnalyzed = false;
  bool _pallorDetected = false;
  
  int selectedGender = 0;
  bool isPregnant = false;
  int pregnancyMonth = 1; // NEW: Track pregnancy month (1-9)

  Map<String, String> symptoms = {
    "Persistent fatigue despite adequate rest": "Common symptom seen in iron-deficiency anemia",
    "Exertional dyspnea (breathlessness on mild activity)": "Indicates reduced oxygen-carrying capacity",
    "Frequent dizziness or light-headedness": "May result from cerebral hypoxia",
    "Cold extremities (hands/feet)": "Peripheral vasoconstriction due to low hemoglobin",
    "Recurrent headaches": "Brain oxygen shortage triggers headaches",
    "Palpitations or tachycardia": "Heart compensates for low oxygen",
    "Difficulty concentrating or brain fog": "Cognitive impairment from reduced oxygen",
    "Excessive daytime somnolence": "Chronic fatigue indicator",
    "Hair loss or brittle nails": "Iron deficiency affects keratin synthesis",
    "Chest discomfort during physical exertion": "Cardiac stress symptom - seek immediate care",
    "Previously diagnosed with anemia": "High recurrence risk",
  };

  Map<String, bool> selectedSymptoms = {};

  @override
  void initState() {
    super.initState();
    symptoms.keys.forEach((key) {
      selectedSymptoms[key] = false;
    });
  }

  Future<void> _pickImage(ImageSource source) async {
    try {
      final XFile? pickedFile = await _picker.pickImage(source: source, maxWidth: 1024, maxHeight: 1024, imageQuality: 85);
      if (pickedFile != null) {
        setState(() {
          _uploadedImage = File(pickedFile.path);
          _imageAnalyzed = false;
        });
        await _analyzePallor();
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Error: $e")));
    }
  }

  Future<void> _analyzePallor() async {
    if (_uploadedImage == null) return;
    try {
      showDialog(context: context, barrierDismissible: false, builder: (context) => Center(child: CircularProgressIndicator()));
      final bytes = await _uploadedImage!.readAsBytes();
      final image = img.decodeImage(bytes);

      if (image != null) {
        int totalR = 0, totalG = 0, totalB = 0, pixelCount = 0;
        int startX = (image.width * 0.3).toInt(), endX = (image.width * 0.7).toInt();
        int startY = (image.height * 0.3).toInt(), endY = (image.height * 0.7).toInt();

        for (int y = startY; y < endY; y += 5) {
          for (int x = startX; x < endX; x += 5) {
            final pixel = image.getPixel(x, y);
            totalR += pixel.r.toInt();
            totalG += pixel.g.toInt();
            totalB += pixel.b.toInt();
            pixelCount++;
          }
        }

        if (pixelCount > 0) {
          double avgR = totalR / pixelCount, avgG = totalG / pixelCount, avgB = totalB / pixelCount;
          double brightness = (avgR + avgG + avgB) / 3;
          double saturation = (avgR - avgG).abs() + (avgG - avgB).abs() + (avgB - avgR).abs();
          bool isPale = brightness > 180 && saturation < 30 && avgR < 200;
          setState(() {
            _pallorDetected = isPale;
            _imageAnalyzed = true;
          });
        }
      }
      Navigator.pop(context);
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text(_pallorDetected ? "Pallor Detected" : "Normal Tone"),
          content: Text(_pallorDetected ? "Added to assessment." : "No pallor detected."),
          actions: [TextButton(child: Text("OK"), onPressed: () => Navigator.pop(context))],
        ),
      );
    } catch (e) {
      Navigator.pop(context);
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Analysis error: $e")));
    }
  }

  void _showImageSourceDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text("Choose Source"),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ListTile(
              leading: Icon(Icons.camera_alt),
              title: Text("Camera"),
              onTap: () {
                Navigator.pop(context);
                _pickImage(ImageSource.camera);
              },
            ),
            ListTile(
              leading: Icon(Icons.photo_library),
              title: Text("Gallery"),
              onTap: () {
                Navigator.pop(context);
                _pickImage(ImageSource.gallery);
              },
            ),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Step 2: Detailed Assessment", style: TextStyle(color: Colors.white)),
        centerTitle: true,
        backgroundColor: Color(0xFF0D47A1),
        iconTheme: IconThemeData(color: Colors.white),
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Section: Demographics
            _buildSectionTitle("👤 Demographics"),
            SizedBox(height: 12),
            
            // Age Input
            TextField(
              controller: ageController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: "Age (years)",
                border: OutlineInputBorder(),
                prefixIcon: Icon(Icons.cake, color: Color(0xFF0D47A1)),
              ),
            ),
            SizedBox(height: 16),
            
            // Gender Selection
            Card(
              elevation: 2,
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
              child: Padding(
                padding: EdgeInsets.all(12),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text("Gender", style: TextStyle(fontSize: 15, fontWeight: FontWeight.w600)),
                    Row(
                      children: [
                        Expanded(
                          child: RadioListTile<int>(
                            title: Text("Female"),
                            value: 0,
                            groupValue: selectedGender,
                            onChanged: (val) => setState(() {
                              selectedGender = val!;
                              // Reset pregnancy when switching to male
                              if (selectedGender == 1) {
                                isPregnant = false;
                              }
                            }),
                            activeColor: Color(0xFF00897B),
                          ),
                        ),
                        Expanded(
                          child: RadioListTile<int>(
                            title: Text("Male"),
                            value: 1,
                            groupValue: selectedGender,
                            onChanged: (val) => setState(() {
                              selectedGender = val!;
                              if (selectedGender == 1) {
                                isPregnant = false;
                              }
                            }),
                            activeColor: Color(0xFF00897B),
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            
            // NEW: Pregnancy Question with Month Dropdown (only for females)
            if (selectedGender == 0) ...[
              SizedBox(height: 12),
              Card(
                elevation: 2,
                color: Colors.pink[50],
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                child: Padding(
                  padding: EdgeInsets.all(12),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      CheckboxListTile(
                        title: Text("Currently Pregnant?", style: TextStyle(fontWeight: FontWeight.w600)),
                        subtitle: Text("Pregnancy affects Hb thresholds", style: TextStyle(fontSize: 12)),
                        value: isPregnant,
                        onChanged: (val) => setState(() => isPregnant = val!),
                        activeColor: Colors.pink,
                        contentPadding: EdgeInsets.zero,
                      ),
                      
                      // Show month dropdown if pregnant
                      if (isPregnant) ...[
                        SizedBox(height: 8),
                        Container(
                          padding: EdgeInsets.symmetric(horizontal: 12),
                          decoration: BoxDecoration(
                            color: Colors.white,
                            borderRadius: BorderRadius.circular(8),
                            border: Border.all(color: Colors.pink.shade200),
                          ),
                          child: DropdownButtonHideUnderline(
                            child: DropdownButton<int>(
                              isExpanded: true,
                              value: pregnancyMonth,
                              icon: Icon(Icons.arrow_drop_down, color: Colors.pink),
                              style: TextStyle(color: Colors.black87, fontSize: 15),
                              items: List.generate(9, (index) {
                                int month = index + 1;
                                String trimester = month <= 3 ? "1st" : month <= 6 ? "2nd" : "3rd";
                                return DropdownMenuItem<int>(
                                  value: month,
                                  child: Text("Month $month ($trimester Trimester)"),
                                );
                              }),
                              onChanged: (val) => setState(() => pregnancyMonth = val!),
                            ),
                          ),
                        ),
                      ],
                    ],
                  ),
                ),
              ),
            ],
            
            SizedBox(height: 20),
            
            // Section: Hemoglobin Input
            _buildSectionTitle("🩸 Hemoglobin Value"),
            SizedBox(height: 12),
            TextField(
              controller: hbController,
              keyboardType: TextInputType.numberWithOptions(decimal: true),
              decoration: InputDecoration(
                labelText: "Hemoglobin (g/dL)",
                border: OutlineInputBorder(),
                prefixIcon: Icon(Icons.opacity, color: Colors.red),
                helperText: "Enter value from blood test report",
              ),
            ),
            
            SizedBox(height: 20),
            
            // Section: Visual Pallor Analysis
            _buildSectionTitle("📸 Visual Pallor Analysis (Optional)"),
            SizedBox(height: 12),
            Card(
              elevation: 2,
              color: Colors.orange[50],
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
              child: Padding(
                padding: EdgeInsets.all(16),
                child: Column(
                  children: [
                    Text(
                      "Upload photo of palm/lower eyelid",
                      style: TextStyle(fontSize: 13, color: Colors.grey[700]),
                    ),
                    SizedBox(height: 12),
                    if (_uploadedImage != null) ...[
                      ClipRRect(
                        borderRadius: BorderRadius.circular(8),
                        child: Image.file(_uploadedImage!, height: 150, width: double.infinity, fit: BoxFit.cover),
                      ),
                      SizedBox(height: 10),
                      if (_imageAnalyzed)
                        Container(
                          padding: EdgeInsets.all(10),
                          decoration: BoxDecoration(
                            color: _pallorDetected ? Colors.orange[100] : Colors.green[100],
                            borderRadius: BorderRadius.circular(6),
                          ),
                          child: Text(
                            _pallorDetected ? "Pallor detected ⚠️" : "Normal ✓",
                            style: TextStyle(fontWeight: FontWeight.bold),
                          ),
                        ),
                      SizedBox(height: 10),
                      ElevatedButton(
                        child: Text("Change Photo"),
                        onPressed: _showImageSourceDialog,
                      ),
                    ] else
                      ElevatedButton.icon(
                        icon: Icon(Icons.add_a_photo),
                        label: Text("Upload Photo"),
                        style: ElevatedButton.styleFrom(
                          minimumSize: Size(double.infinity, 50),
                          backgroundColor: Color(0xFFFF6F00),
                          foregroundColor: Colors.white,
                          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                        ),
                        onPressed: _showImageSourceDialog,
                      ),
                  ],
                ),
              ),
            ),
            
            SizedBox(height: 24),
            
            // Section: Clinical Symptoms
            _buildSectionTitle("🩺 Clinical Symptom Assessment"),
            SizedBox(height: 8),
            Text(
              "Select all symptoms that apply",
              style: TextStyle(fontSize: 13, color: Colors.grey[600]),
            ),
            SizedBox(height: 12),

            ...symptoms.entries.map((entry) {
              return Card(
                margin: EdgeInsets.only(bottom: 8),
                elevation: 1,
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                child: CheckboxListTile(
                  title: Text(
                    entry.key,
                    style: TextStyle(fontSize: 14, fontWeight: FontWeight.w500),
                  ),
                  subtitle: Text(
                    entry.value,
                    style: TextStyle(fontSize: 12, color: Colors.grey[600], fontStyle: FontStyle.italic),
                  ),
                  value: selectedSymptoms[entry.key],
                  onChanged: (val) => setState(() => selectedSymptoms[entry.key] = val!),
                  activeColor: Color(0xFF00897B),
                  controlAffinity: ListTileControlAffinity.leading,
                ),
              );
            }).toList(),

            SizedBox(height: 30),
            
            Center(
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(
                  backgroundColor: Color(0xFF0D47A1),
                  foregroundColor: Colors.white,
                  padding: EdgeInsets.symmetric(horizontal: 60, vertical: 18),
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
                  elevation: 3,
                ),
                child: Text("Analyze Results", style: TextStyle(fontSize: 17, fontWeight: FontWeight.w600)),
                onPressed: () {
                  int age = int.tryParse(ageController.text) ?? 0;
                  double hb = double.tryParse(hbController.text) ?? 0;
                  
                  if (age == 0) {
                    ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Please enter age")));
                    return;
                  }
                  
                  if (hb == 0) {
                    ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Please enter Hb value")));
                    return;
                  }

                  int symptomScore = selectedSymptoms.values.where((v) => v).length;
                  if (_pallorDetected) symptomScore += 1;

                  var assessment = AssessmentResult.calculateRisk(
                    hb: hb,
                    age: age,
                    gender: selectedGender,
                    isPregnant: isPregnant,
                    pregnancyMonth: pregnancyMonth, // NEW: Pass pregnancy month
                    symptomScore: symptomScore,
                    pallorDetected: _pallorDetected,
                  );

                  String pregnancyInfo = "";
                  if (isPregnant) {
                    String trimester = pregnancyMonth <= 3 ? "1st" : pregnancyMonth <= 6 ? "2nd" : "3rd";
                    pregnancyInfo = " (Pregnant - Month $pregnancyMonth, $trimester Trimester)";
                  }

                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => ResultScreen(
                        result: assessment['result'],
                        foodsToAvoid: assessment['foodsToAvoid'],
                        clinicalRecommendation: assessment['clinicalRecommendation'],
                        whyThisResult: assessment['whyThisResult'],
                        riskScore: assessment['riskScore'],
                        details: "Age: $age years\nGender: ${selectedGender == 0 ? 'Female' : 'Male'}$pregnancyInfo\nHb: ${hb.toStringAsFixed(1)} g/dL\nSymptoms: $symptomScore/12${_pallorDetected ? ' (including pallor)' : ''}",
                        uploadedImage: _uploadedImage,
                      ),
                    ),
                  );
                },
              ),
            ),
            SizedBox(height: 20),
          ],
        ),
      ),
    );
  }

  Widget _buildSectionTitle(String title) {
    return Text(title, style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Color(0xFF0D47A1)));
  }
}

// ================= PATH 2: WITHOUT HB DATA (SYMPTOM-BASED ONLY) =================
class WithoutHbScreen extends StatefulWidget {
  @override
  _WithoutHbScreenState createState() => _WithoutHbScreenState();
}

class _WithoutHbScreenState extends State<WithoutHbScreen> {
  final TextEditingController ageController = TextEditingController(); // NEW
  final ImagePicker _picker = ImagePicker();
  
  File? _uploadedImage;
  bool _imageAnalyzed = false;
  bool _pallorDetected = false;
  
  int selectedGender = 0;
  bool isPregnant = false;
  int pregnancyMonth = 1; // NEW: Also add for symptom-based path

  Map<String, String> symptoms = {
    "Persistent fatigue despite adequate rest": "Common symptom seen in iron-deficiency anemia",
    "Exertional dyspnea (breathlessness on mild activity)": "Indicates reduced oxygen-carrying capacity",
    "Frequent dizziness or light-headedness": "May result from cerebral hypoxia",
    "Cold extremities (hands/feet)": "Peripheral vasoconstriction due to low hemoglobin",
    "Recurrent headaches": "Brain oxygen shortage triggers headaches",
    "Palpitations or tachycardia": "Heart compensates for low oxygen",
    "Difficulty concentrating or brain fog": "Cognitive impairment from reduced oxygen",
    "Excessive daytime somnolence": "Chronic fatigue indicator",
    "Hair loss or brittle nails": "Iron deficiency affects keratin synthesis",
    "Chest discomfort during physical exertion": "Cardiac stress symptom - seek immediate care",
    "Previously diagnosed with anemia": "High recurrence risk",
  };

  Map<String, bool> selectedSymptoms = {};

  @override
  void initState() {
    super.initState();
    symptoms.keys.forEach((key) {
      selectedSymptoms[key] = false;
    });
  }

  Future<void> _pickImage(ImageSource source) async {
    try {
      final XFile? pickedFile = await _picker.pickImage(source: source, maxWidth: 1024, maxHeight: 1024, imageQuality: 85);
      if (pickedFile != null) {
        setState(() {
          _uploadedImage = File(pickedFile.path);
          _imageAnalyzed = false;
        });
        await _analyzePallor();
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Error: $e")));
    }
  }

  Future<void> _analyzePallor() async {
    if (_uploadedImage == null) return;
    try {
      showDialog(context: context, barrierDismissible: false, builder: (context) => Center(child: CircularProgressIndicator()));
      final bytes = await _uploadedImage!.readAsBytes();
      final image = img.decodeImage(bytes);

      if (image != null) {
        int totalR = 0, totalG = 0, totalB = 0, pixelCount = 0;
        int startX = (image.width * 0.3).toInt(), endX = (image.width * 0.7).toInt();
        int startY = (image.height * 0.3).toInt(), endY = (image.height * 0.7).toInt();

        for (int y = startY; y < endY; y += 5) {
          for (int x = startX; x < endX; x += 5) {
            final pixel = image.getPixel(x, y);
            totalR += pixel.r.toInt();
            totalG += pixel.g.toInt();
            totalB += pixel.b.toInt();
            pixelCount++;
          }
        }

        if (pixelCount > 0) {
          double avgR = totalR / pixelCount, avgG = totalG / pixelCount, avgB = totalB / pixelCount;
          double brightness = (avgR + avgG + avgB) / 3;
          double saturation = (avgR - avgG).abs() + (avgG - avgB).abs() + (avgB - avgR).abs();
          bool isPale = brightness > 180 && saturation < 30 && avgR < 200;
          setState(() {
            _pallorDetected = isPale;
            _imageAnalyzed = true;
          });
        }
      }
      Navigator.pop(context);
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text(_pallorDetected ? "Pallor Detected" : "Normal Tone"),
          content: Text(_pallorDetected ? "Added to assessment." : "No pallor detected."),
          actions: [TextButton(child: Text("OK"), onPressed: () => Navigator.pop(context))],
        ),
      );
    } catch (e) {
      Navigator.pop(context);
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Analysis error: $e")));
    }
  }

  void _showImageSourceDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text("Choose Source"),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ListTile(
              leading: Icon(Icons.camera_alt),
              title: Text("Camera"),
              onTap: () {
                Navigator.pop(context);
                _pickImage(ImageSource.camera);
              },
            ),
            ListTile(
              leading: Icon(Icons.photo_library),
              title: Text("Gallery"),
              onTap: () {
                Navigator.pop(context);
                _pickImage(ImageSource.gallery);
              },
            ),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Step 2: Symptom Assessment", style: TextStyle(color: Colors.white)),
        centerTitle: true,
        backgroundColor: Color(0xFF0D47A1),
        iconTheme: IconThemeData(color: Colors.white),
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              padding: EdgeInsets.all(14),
              decoration: BoxDecoration(
                color: Colors.orange[50],
                borderRadius: BorderRadius.circular(10),
                border: Border.all(color: Colors.orange[300]!, width: 1.5),
              ),
              child: Row(
                children: [
                  Icon(Icons.info, color: Colors.orange[900]),
                  SizedBox(width: 12),
                  Expanded(
                    child: Text(
                      "No Hb data available. Assessment will be symptom-based only.",
                      style: TextStyle(fontSize: 13, color: Colors.orange[900], fontWeight: FontWeight.w500),
                    ),
                  ),
                ],
              ),
            ),
            
            SizedBox(height: 20),
            
            // Section: Demographics
            _buildSectionTitle("👤 Demographics"),
            SizedBox(height: 12),
            
            // Age Input
            TextField(
              controller: ageController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: "Age (years)",
                border: OutlineInputBorder(),
                prefixIcon: Icon(Icons.cake, color: Color(0xFF0D47A1)),
              ),
            ),
            SizedBox(height: 16),
            
            // Gender Selection
            Card(
              elevation: 2,
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
              child: Padding(
                padding: EdgeInsets.all(12),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text("Gender", style: TextStyle(fontSize: 15, fontWeight: FontWeight.w600)),
                    Row(
                      children: [
                        Expanded(
                          child: RadioListTile<int>(
                            title: Text("Female"),
                            value: 0,
                            groupValue: selectedGender,
                            onChanged: (val) => setState(() {
                              selectedGender = val!;
                              if (selectedGender == 1) {
                                isPregnant = false;
                              }
                            }),
                            activeColor: Color(0xFF00897B),
                          ),
                        ),
                        Expanded(
                          child: RadioListTile<int>(
                            title: Text("Male"),
                            value: 1,
                            groupValue: selectedGender,
                            onChanged: (val) => setState(() {
                              selectedGender = val!;
                              if (selectedGender == 1) {
                                isPregnant = false;
                              }
                            }),
                            activeColor: Color(0xFF00897B),
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            
            // NEW: Pregnancy Question with Month Dropdown (only for females)
            if (selectedGender == 0) ...[
              SizedBox(height: 12),
              Card(
                elevation: 2,
                color: Colors.pink[50],
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                child: Padding(
                  padding: EdgeInsets.all(12),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      CheckboxListTile(
                        title: Text("Currently Pregnant?", style: TextStyle(fontWeight: FontWeight.w600)),
                        subtitle: Text("Important for risk assessment", style: TextStyle(fontSize: 12)),
                        value: isPregnant,
                        onChanged: (val) => setState(() => isPregnant = val!),
                        activeColor: Colors.pink,
                        contentPadding: EdgeInsets.zero,
                      ),
                      
                      // Show month dropdown if pregnant
                      if (isPregnant) ...[
                        SizedBox(height: 8),
                        Container(
                          padding: EdgeInsets.symmetric(horizontal: 12),
                          decoration: BoxDecoration(
                            color: Colors.white,
                            borderRadius: BorderRadius.circular(8),
                            border: Border.all(color: Colors.pink.shade200),
                          ),
                          child: DropdownButtonHideUnderline(
                            child: DropdownButton<int>(
                              isExpanded: true,
                              value: pregnancyMonth,
                              icon: Icon(Icons.arrow_drop_down, color: Colors.pink),
                              style: TextStyle(color: Colors.black87, fontSize: 15),
                              items: List.generate(9, (index) {
                                int month = index + 1;
                                String trimester = month <= 3 ? "1st" : month <= 6 ? "2nd" : "3rd";
                                return DropdownMenuItem<int>(
                                  value: month,
                                  child: Text("Month $month ($trimester Trimester)"),
                                );
                              }),
                              onChanged: (val) => setState(() => pregnancyMonth = val!),
                            ),
                          ),
                        ),
                      ],
                    ],
                  ),
                ),
              ),
            ],
            
            SizedBox(height: 24),
            
            // Section: Visual Pallor Analysis
            _buildSectionTitle("📸 Visual Pallor Analysis (Optional)"),
            SizedBox(height: 12),
            Card(
              elevation: 2,
              color: Colors.orange[50],
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
              child: Padding(
                padding: EdgeInsets.all(16),
                child: Column(
                  children: [
                    Text(
                      "Upload photo of palm/lower eyelid",
                      style: TextStyle(fontSize: 13, color: Colors.grey[700]),
                    ),
                    SizedBox(height: 12),
                    if (_uploadedImage != null) ...[
                      ClipRRect(
                        borderRadius: BorderRadius.circular(8),
                        child: Image.file(_uploadedImage!, height: 150, width: double.infinity, fit: BoxFit.cover),
                      ),
                      SizedBox(height: 10),
                      if (_imageAnalyzed)
                        Container(
                          padding: EdgeInsets.all(10),
                          decoration: BoxDecoration(
                            color: _pallorDetected ? Colors.orange[100] : Colors.green[100],
                            borderRadius: BorderRadius.circular(6),
                          ),
                          child: Text(
                            _pallorDetected ? "Pallor detected ⚠️" : "Normal ✓",
                            style: TextStyle(fontWeight: FontWeight.bold),
                          ),
                        ),
                      SizedBox(height: 10),
                      ElevatedButton(
                        child: Text("Change Photo"),
                        onPressed: _showImageSourceDialog,
                      ),
                    ] else
                      ElevatedButton.icon(
                        icon: Icon(Icons.add_a_photo),
                        label: Text("Upload Photo"),
                        style: ElevatedButton.styleFrom(
                          minimumSize: Size(double.infinity, 50),
                          backgroundColor: Color(0xFFFF6F00),
                          foregroundColor: Colors.white,
                          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                        ),
                        onPressed: _showImageSourceDialog,
                      ),
                  ],
                ),
              ),
            ),
            
            SizedBox(height: 24),
            
            // Section: Clinical Symptoms
            _buildSectionTitle("🩺 Clinical Symptom Assessment"),
            SizedBox(height: 8),
            Text(
              "Select all symptoms that apply",
              style: TextStyle(fontSize: 13, color: Colors.grey[600]),
            ),
            SizedBox(height: 12),

            ...symptoms.entries.map((entry) {
              return Card(
                margin: EdgeInsets.only(bottom: 8),
                elevation: 1,
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
                child: CheckboxListTile(
                  title: Text(
                    entry.key,
                    style: TextStyle(fontSize: 14, fontWeight: FontWeight.w500),
                  ),
                  subtitle: Text(
                    entry.value,
                    style: TextStyle(fontSize: 12, color: Colors.grey[600], fontStyle: FontStyle.italic),
                  ),
                  value: selectedSymptoms[entry.key],
                  onChanged: (val) => setState(() => selectedSymptoms[entry.key] = val!),
                  activeColor: Color(0xFF00897B),
                  controlAffinity: ListTileControlAffinity.leading,
                ),
              );
            }).toList(),

            SizedBox(height: 30),
            Center(
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(
                  backgroundColor: Color(0xFF0D47A1),
                  foregroundColor: Colors.white,
                  padding: EdgeInsets.symmetric(horizontal: 60, vertical: 18),
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
                ),
                child: Text("Analyze Risk", style: TextStyle(fontSize: 17, fontWeight: FontWeight.w600)),
                onPressed: () {
                  // NEW: Age validation
                  int age = int.tryParse(ageController.text) ?? 0;
                  if (age == 0) {
                    ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Please enter age")));
                    return;
                  }
                  
                  int score = selectedSymptoms.values.where((v) => v).length;
                  if (_pallorDetected) score += 1;

                  String result, foodsToAvoid, clinicalRec;
                  double riskScore;

                  if (score <= 4) {
                    result = "🟢 Low Risk";
                    riskScore = score / 12.0;
                    foodsToAvoid = "Limit with Iron-Rich Meals:\n• Tea & Coffee\n• Milk\n• White bread\n\nEncouraged:\n• Vitamin-C rich foods\n• Green leafy vegetables";
                    clinicalRec = "• Maintain iron-rich balanced diet\n• Repeat Hb test annually\n• Monitor symptoms if fatigue increases";
                  } else if (score <= 9) {
                    result = "🟡 Moderate Risk";
                    riskScore = score / 12.0;
                    foodsToAvoid = "Strictly Avoid:\n• Tea/Coffee near meals\n• Dairy with iron\n• Soft drinks\n• Fried foods";
                    clinicalRec = "• Suggested CBC test\n• Dietary iron correction\n• Doctor consultation advised";
                  } else {
                    result = "🔴 High Risk";
                    riskScore = score / 12.0;
                    foodsToAvoid = "COMPLETELY AVOID:\n• Tea/Coffee\n• Milk with supplements\n• High-fiber at med time\n• Fast food";
                    clinicalRec = "• Immediate blood test recommended\n• Possible iron/B12 deficiency\n• Medical supervision required";
                  }

                  String pregnancyInfo = "";
                  if (isPregnant) {
                    String trimester = pregnancyMonth <= 3 ? "1st" : pregnancyMonth <= 6 ? "2nd" : "3rd";
                    pregnancyInfo = " (Pregnant - Month $pregnancyMonth, $trimester Trimester)";
                  }

                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => ResultScreen(
                        result: result,
                        foodsToAvoid: foodsToAvoid,
                        clinicalRecommendation: clinicalRec,
                        whyThisResult: "Based on symptom analysis",
                        riskScore: riskScore,
                        // NEW: Include demographics in details
                        details: "Age: $age years\nGender: ${selectedGender == 0 ? 'Female' : 'Male'}$pregnancyInfo\nSymptoms: $score/12${_pallorDetected ? ' (pallor detected)' : ''}",
                        uploadedImage: _uploadedImage,
                      ),
                    ),
                  );
                },
              ),
            ),
            SizedBox(height: 20),
          ],
        ),
      ),
    );
  }

  Widget _buildSectionTitle(String title) {
    return Text(title, style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Color(0xFF0D47A1)));
  }
}

// ================= ASSESSMENT RESULT CALCULATION (UPDATED WITH PREGNANCY MONTH LOGIC) =================
class AssessmentResult {
  static Map<String, dynamic> calculateRisk({
    required double hb,
    required int age,
    required int gender,
    required bool isPregnant,
    required int pregnancyMonth, // NEW parameter
    required int symptomScore,
    required bool pallorDetected,
  }) {
    // NEW: Calculate normal Hb threshold based on pregnancy month
    double normalThreshold;
    String anemiaRiskLevel = "";
    
    if (gender == 0) { // Female
      if (isPregnant) {
        // Calculate threshold based on pregnancy month
        if (pregnancyMonth <= 3) {
          // 1st Trimester (1-3 months): 11-13 g/dL
          normalThreshold = 11.0;
          if (hb < 11.0) {
            anemiaRiskLevel = pregnancyMonth == 1 || pregnancyMonth == 2 ? "Low" : "Low–Medium";
          }
        } else if (pregnancyMonth <= 6) {
          // 2nd Trimester (4-6 months): 10.5-12 g/dL
          normalThreshold = 10.5;
          if (pregnancyMonth == 4) {
            anemiaRiskLevel = hb < 10.5 ? "Medium" : "";
          } else if (pregnancyMonth == 5) {
            anemiaRiskLevel = hb < 10.5 ? "Medium" : "";
          } else { // month 6
            anemiaRiskLevel = hb < 10.5 ? "High" : "";
          }
        } else {
          // 3rd Trimester (7-9 months): 11-12 g/dL
          normalThreshold = 11.0;
          anemiaRiskLevel = hb < 11.0 ? "High" : "";
        }
      } else {
        // Non-pregnant female
        normalThreshold = 12.0;
      }
    } else { // Male
      normalThreshold = 13.0;
    }
    
    String anemiaType, foodsToAvoid, clinicalRecommendation, whyThisResult;
    
    // Build pregnancy-specific info string if applicable
    String pregnancyNote = "";
    if (isPregnant) {
      String trimester = pregnancyMonth <= 3 ? "1st" : pregnancyMonth <= 6 ? "2nd" : "3rd";
      String normalRange = "";
      if (pregnancyMonth <= 3) {
        normalRange = "11-13 g/dL";
      } else if (pregnancyMonth <= 6) {
        if (pregnancyMonth == 4) normalRange = "10.5-12 g/dL";
        else if (pregnancyMonth == 5) normalRange = "10.5-11.5 g/dL";
        else normalRange = "10.5-11 g/dL";
      } else {
        normalRange = "11-12 g/dL";
      }
      pregnancyNote = "\n\n🤰 Pregnancy Info:\n• Month: $pregnancyMonth ($trimester Trimester)\n• Normal Hb range: $normalRange\n• Anemia Risk: $anemiaRiskLevel";
    }
    
    if (hb >= normalThreshold) {
      anemiaType = "🟢 Normal/Borderline";
      whyThisResult = "✅ Hemoglobin within normal range\n${symptomScore > 5 ? '⚠️ However, ${symptomScore} symptoms reported - further investigation advised' : '✓ Minimal symptoms reported'}${pallorDetected ? '\n⚠️ Visual pallor noted' : ''}$pregnancyNote";
      
      // Enhanced dietary guidance
      foodsToAvoid = """🍽️ Preventive Measures:

❌ Foods to LIMIT/AVOID with iron-rich meals:
• Tea & Coffee (blocks iron absorption)
  👉 Drink 2 hours after meals if needed
• Milk, Curd, Cheese (calcium stops iron)
  👉 Take 1-2 hours after iron-rich food
• Chocolate & Cocoa (reduces absorption)
• Excessive refined foods (white bread, maida)

✅ Focus on vitamin C for iron absorption
✅ Green leafy vegetables
✅ Whole grains over refined""";

      clinicalRecommendation = "Clinical Recommendation:\n• Monitor annually if symptomatic\n• Maintain balanced diet\n• No immediate intervention needed\n• Recheck Hb if symptoms worsen${isPregnant ? '\n• Regular prenatal check-ups essential' : ''}";
    } else if (hb >= 10.0) {
      anemiaType = "🟡 Mild Anemia";
      whyThisResult = "⚠️ Hemoglobin slightly below normal\n⚠️ ${symptomScore} symptoms reported\n${pallorDetected ? '⚠️ Visual pallor present' : ''}\n⚠️ Dietary correction recommended$pregnancyNote";
      
      // Enhanced dietary guidance
      foodsToAvoid = """🍽️ AVOID THESE:

❌ Foods NOT to take (or take carefully):
☕ Tea & Coffee
   • Reduce or avoid (blocks iron)
   • If needed, drink 2 hours after meals

🥛 Milk, Curd, Cheese (with meals)
   • Don't take with iron-rich food or iron tablets
   • Calcium stops iron absorption
   • Can take after 1-2 hours

🍫 Chocolate & Cocoa
   • Limit (reduces iron absorption)

🍟 Junk & Fast Foods
   • Chips, burgers, pizza, instant noodles
   • No iron, causes poor nutrition

🧂 Too Much Salt
   • Pickles, papad, packaged foods
   • Can increase BP & reduce nutrient balance

⚠️ May worsen iron absorption!""";

      clinicalRecommendation = "Clinical Recommendation:\n• Dietary iron supplementation\n• Follow-up Hb test in 3 months\n• Doctor consultation recommended\n• Monitor symptom progression${isPregnant ? '\n• Iron supplements often prescribed during pregnancy\n• Prenatal vitamins essential' : ''}";
    } else if (hb >= 7.0) {
      anemiaType = "🟠 Moderate Anemia";
      whyThisResult = "🚨 Hemoglobin significantly low\n🚨 ${symptomScore} symptoms reported\n${pallorDetected ? '🚨 Visual pallor present' : ''}\n🚨 Medical intervention required$pregnancyNote";
      
      // Enhanced dietary guidance
      foodsToAvoid = """🍽️ STRICTLY AVOID:

❌ Foods NOT to take (or take carefully):
☕ Tea & Coffee
   • COMPLETELY AVOID (blocks iron absorption)

🥛 Milk, Curd, Cheese
   • NEVER with iron tablets
   • Calcium blocks iron completely

🍫 Chocolate & Cocoa
   • Strictly limit

🍟 Junk & Fast Foods
   • Chips, burgers, pizza, instant noodles
   • Zero nutritional value

🧂 Too Much Salt
   • Pickles, papad, packaged foods

🍰 Excess Sugar & Sweets
   • Cakes, pastries, soft drinks
   • Fills stomach, no nutrition

🌾 Refined Foods
   • White bread, maida items
   • Low iron vs whole grains

🚨 Consult doctor immediately!""";

      clinicalRecommendation = "Clinical Recommendation:\n• Immediate medical consultation\n• Iron/B12 supplementation likely\n• CBC + serum ferritin test\n• Rule out chronic bleeding${isPregnant ? '\n• URGENT prenatal care required\n• Risk to mother and baby\n• Possible IV iron therapy' : ''}";
    } else {
      anemiaType = "🔴 Severe Anemia";
      whyThisResult = "🚨 Hemoglobin critically low\n🚨 ${symptomScore} symptoms reported\n${pallorDetected ? '🚨 Visual pallor present' : ''}\n🚨 URGENT medical attention required$pregnancyNote";
      
      // Enhanced dietary guidance
      foodsToAvoid = """🍽️ COMPLETELY AVOID:

❌ Foods NOT to take (CRITICAL):
☕ Tea & Coffee
   • ZERO tolerance - blocks iron completely

🥛 Milk, Curd, Cheese
   • NEVER with iron tablets
   • Separate by 2+ hours minimum

🍫 Chocolate & Cocoa
   • Complete avoidance

🍟 Junk & Fast Foods
   • All chips, burgers, pizza, instant noodles
   • Dangerous for your condition

🧂 Too Much Salt
   • All pickles, papad, packaged foods

🍰 Excess Sugar & Sweets
   • All cakes, pastries, soft drinks

🌾 Refined Foods
   • All white bread, maida items

🚨 Food alone is NOT enough!
🚨 Medical treatment MANDATORY!""";

      clinicalRecommendation = "Clinical Recommendation:\n• EMERGENCY blood test required\n• Immediate hospitalization may be needed\n• Possible iron/B12 deficiency\n• Medical supervision MANDATORY\n• Urgent treatment needed${isPregnant ? '\n• CRITICAL - Risk to mother and baby life\n• Immediate hospitalization likely\n• Blood transfusion may be required\n• Contact emergency obstetric care NOW' : ''}";
    }
    
    double riskScore = (hb >= normalThreshold) ? 0.0 : (hb >= 10.0) ? 0.25 : (hb >= 7.0) ? 0.60 : 1.0;
    if (symptomScore > 7 || pallorDetected) riskScore = (riskScore + 0.15).clamp(0.0, 1.0);
    
    // Adjust risk score for pregnancy
    if (isPregnant && hb < normalThreshold) {
      riskScore = (riskScore + 0.10).clamp(0.0, 1.0);
    }
    
    return {
      'result': anemiaType,
      'foodsToAvoid': foodsToAvoid,
      'clinicalRecommendation': clinicalRecommendation,
      'whyThisResult': whyThisResult,
      'riskScore': riskScore,
    };
  }
}

// ================= RESULT SCREEN (UPGRADED) =================
class ResultScreen extends StatelessWidget {
  final String result, foodsToAvoid, clinicalRecommendation, whyThisResult, details;
  final double riskScore;
  final File? uploadedImage;

  ResultScreen({
    required this.result,
    required this.foodsToAvoid,
    required this.clinicalRecommendation,
    required this.whyThisResult,
    required this.riskScore,
    required this.details,
    this.uploadedImage,
  });

  Color getColor() {
    if (result.contains("🔴")) return Color(0xFFD32F2F);
    if (result.contains("🟠")) return Color(0xFFFF6F00);
    if (result.contains("🟡")) return Color(0xFFFFA000);
    return Color(0xFF00897B);
  }

  String getRiskCategory() {
    if (riskScore <= 0.15) return "🟢 Low Risk (0-15%)";
    if (riskScore <= 0.40) return "🟡 Moderate Risk (16-40%)";
    return "🔴 High Risk (41%+)";
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Step 3: Risk Analysis", style: TextStyle(color: Colors.white)),
        centerTitle: true,
        backgroundColor: Color(0xFF0D47A1),
        iconTheme: IconThemeData(color: Colors.white),
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(20),
        child: Column(
          children: [
            SizedBox(height: 10),
            Container(
              padding: EdgeInsets.all(20),
              decoration: BoxDecoration(
                color: getColor().withOpacity(0.1),
                shape: BoxShape.circle,
              ),
              child: Icon(
                result.contains("🔴") || result.contains("🟠") ? Icons.warning : Icons.check_circle,
                size: 70,
                color: getColor(),
              ),
            ),
            SizedBox(height: 20),
            Text(
              result,
              style: TextStyle(fontSize: 28, fontWeight: FontWeight.bold, color: getColor()),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 10),
            Text(
              getRiskCategory(),
              style: TextStyle(fontSize: 16, color: Colors.grey[700], fontWeight: FontWeight.w500),
            ),
            SizedBox(height: 30),
            
            if (uploadedImage != null) ...[
              Card(
                elevation: 2,
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
                child: Padding(
                  padding: EdgeInsets.all(12),
                  child: Column(
                    children: [
                      Text("Visual Analysis", style: TextStyle(fontWeight: FontWeight.bold, fontSize: 15)),
                      SizedBox(height: 10),
                      ClipRRect(
                        borderRadius: BorderRadius.circular(8),
                        child: Image.file(uploadedImage!, height: 140, width: double.infinity, fit: BoxFit.cover),
                      ),
                    ],
                  ),
                ),
              ),
              SizedBox(height: 20),
            ],
            
            Card(
              elevation: 3,
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
              child: Padding(
                padding: EdgeInsets.all(18),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text("Risk Stratification Model", style: TextStyle(fontSize: 17, fontWeight: FontWeight.bold, color: Color(0xFF0D47A1))),
                    SizedBox(height: 12),
                    LinearProgressIndicator(
                      value: riskScore,
                      backgroundColor: Colors.grey[300],
                      valueColor: AlwaysStoppedAnimation<Color>(getColor()),
                      minHeight: 12,
                    ),
                    SizedBox(height: 8),
                    Text("${(riskScore * 100).toStringAsFixed(0)}% Risk Level", style: TextStyle(fontSize: 16, fontWeight: FontWeight.w600, color: getColor())),
                  ],
                ),
              ),
            ),
            
            SizedBox(height: 20),
            
            _buildInfoCard(
              "📊 Details",
              details,
              Colors.grey[100]!,
            ),
            
            SizedBox(height: 15),
            
            _buildInfoCard(
              "🔍 Why This Result?",
              whyThisResult,
              Colors.blue[50]!,
            ),
            
            SizedBox(height: 15),
            
            _buildInfoCard(
              "🩺 Clinical Recommendation",
              clinicalRecommendation,
              getColor().withOpacity(0.1),
            ),
            
            SizedBox(height: 15),
            
            _buildInfoCard(
              "🍽️ Dietary Guidance",
              foodsToAvoid,
              Colors.orange[50]!,
            ),
            
            SizedBox(height: 25),
            
            Container(
              padding: EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Color(0xFF0D47A1).withOpacity(0.1),
                borderRadius: BorderRadius.circular(10),
                border: Border.all(color: Color(0xFF0D47A1), width: 1.5),
              ),
              child: Row(
                children: [
                  Icon(Icons.info, color: Color(0xFF0D47A1)),
                  SizedBox(width: 12),
                  Expanded(
                    child: Text(
                      "🤖 AI Logic Used:\n• Symptom scoring model\n• Hemoglobin threshold analysis\n• Gender & pregnancy-based correction\n• Pregnancy month-specific thresholds\n• Optional visual pallor input",
                      style: TextStyle(fontSize: 12, color: Color(0xFF0D47A1), fontWeight: FontWeight.w500, height: 1.5),
                    ),
                  ),
                ],
              ),
            ),
            
            SizedBox(height: 25),
            
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                backgroundColor: Color(0xFF0D47A1),
                foregroundColor: Colors.white,
                padding: EdgeInsets.symmetric(horizontal: 50, vertical: 16),
                shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
              ),
              child: Text("New Assessment", style: TextStyle(fontSize: 16, fontWeight: FontWeight.w600)),
              onPressed: () => Navigator.popUntil(context, (route) => route.isFirst),
            ),
            SizedBox(height: 20),
          ],
        ),
      ),
    );
  }

  Widget _buildInfoCard(String title, String content, Color bgColor) {
    return Card(
      elevation: 2,
      color: bgColor,
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(title, style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold, color: Color(0xFF0D47A1))),
            SizedBox(height: 10),
            Text(content, style: TextStyle(fontSize: 14, height: 1.5)),
          ],
        ),
      ),
    );
  }
}
