# Walkthrough: FeedForward Platform Implementation & Verification

The **FeedForward** platform has been built, tested, and deployed locally to prevent food overproduction and enable circular waste redistribution across hostels, event catering, and waste processing officials.

---

## 🚀 What Was Built

### 1. Multi-Partition Architecture & Portals
- **🎓 Hosteller / Student Portal**:
  - Pre-meal menu announcements with deadline countdowns.
  - 100% attendance commitment RSVP: "Yes, I will eat" vs "No, I'm skipping".
  - Dietary selection: Pure Vegetarian vs Non-Vegetarian.
  - Item-level quantity customizer: specifying exact counts (e.g. 3 Idlis, 2 Chapattis, 1 Rice bowl).
  - Digital Canteen Entry Pass with QR code and meal ticket breakdown.
- **⭐ Black Star Penalty System**:
  - Turnstile & disposal enforcement: Hostellers who RSVP "Yes" but fail to enter (No-Show), or waste plate food receive a **Black Star**.
  - **3-Star Fine Trigger**: When a student accumulates **3 Black Stars**, a **₹100** penalty fine is automatically issued to their hostel account, accompanied by high-priority warning notifications.
  - Includes fine payment with automatic star reset.
- **👨‍🍳 Kitchen Head Chef Demand Dashboard**:
  - Aggregated real-time metrics: Confirmed diners, Veg vs Non-Veg breakdown, opted-out count, and estimated food saved from overproduction.
  - **Exact Cooking Quota**: Tells the kitchen exact piece counts (e.g., 255 Idlis, 180 Chapattis, 65 Rice bowls).
- **🚪 Canteen Gatekeeper Terminal**:
  - Student QR/ID scan verification against meal reservations.
  - Plate waste logger & penalty desk with 1-click strike enforcement.
- **⚡ Biogas & 🐾 Animal Feed Logistics Hub**:
  - **Circular Waste Routing**:
    - Clean plate food scrap -> Diverted to **Animal Shelters / Cattle Farms**.
    - Spoilt / contaminated / sour food -> Diverted to **Biogas Anaerobic Digestion Plants**.
  - Automated real-time notifications dispatched to respective officials upon logging waste.
  - Tracking lifecycle: *Pending Pickup* → *Dispatched* → *Collected*.

---

## 🧪 Verification & Automated Test Results

The backend automated test suite (`tests/test_backend.py`) verifies all business rules:

```bash
& "C:\Users\srira\AppData\Local\Microsoft\WindowsApps\python.exe" tests/test_backend.py
```

### Test Output
```
--- Running FeedForward Backend Verification Tests ---
[OK] test_get_meals PASSED
[OK] test_student_rsvp_and_chef_demand PASSED: Chef prep sheet shows 10 Idlis, 7 Chapattis
[OK] test_canteen_entry_verification PASSED
[OK] test_black_star_and_fine_trigger PASSED: Reached 3 stars -> Fine levied Rs 100.0
[OK] test_fine_notification PASSED
[OK] test_fine_payment_and_star_reset PASSED: Black stars reset to 0 after clearance
[OK] test_biogas_official_notification PASSED: Urgent alert received by Biogas official
[OK] test_waste_analytics PASSED: Biogas diverted: 133.5 kg (66.75 kWh green energy), Animal Feed: 71.5 kg (178 animal meals)

[SUCCESS] ALL BACKEND TEST CASES PASSED PERFECTLY!
```

---

## 🌐 Live Server Verification

The backend server was started and verified live on `http://127.0.0.1:8000/`:

1. **HTTP Root Check**:
   - `GET http://127.0.0.1:8000/` returned **HTTP 200** with the full Single Page Application HTML shell.
2. **API Endpoint Check**:
   - `GET /api/meals` returned active meal menus with Veg and Non-Veg item schemas.
   - `GET /api/chef/demand/meal_hostel_lunch` returned real-time prep sheets:
     - 10 Idlis, 7 Chapattis, 2 Steamed Rice, 2 Paneer Butter Masala, 1 Chicken Curry.
3. **Static Assets Serving**:
   - `GET /static/js/app.js` and `GET /static/css/style.css` returned **HTTP 200**.

---

## 🖥️ How to Run & Use the Application

### 1. Start Server
Run either launcher from the `feedforward` folder:
- **Windows Command Prompt**: Double-click [run.bat](file:///c:/Users/srira/OneDrive/Documents/feedforward/run.bat)
- **PowerShell**: Run [run.ps1](file:///c:/Users/srira/OneDrive/Documents/feedforward/run.ps1)

### 2. Open in Browser
Navigate to:
```
http://localhost:8000/
```

### 3. Interactive Walkthrough Flow:
1. **As Hosteller (Rahul Sharma or Sneha Reddy)**:
   - Notice the upcoming meal card and Black Star meter.
   - Toggle "Yes, I will eat", choose "Non-Veg", and adjust quantity sliders (e.g. 4 Idlis, 3 Chapattis).
   - Click "Confirm Selection & Update Canteen Entry Pass".
2. **As Head Chef (Chef Ramesh Gupta)**:
   - Switch to the "Head Chef" tab in the top navigation.
   - Notice the live count immediately updates with exact item counts to cook.
3. **As Canteen Gatekeeper (Officer David)**:
   - Select the student and click "Verify & Check In Student" to approve entry.
   - Test penalty: Select a student, pick "Excessive plate food waste", and click "Issue Black Star Strike".
   - When 3 stars are reached, a fine banner alerts the gatekeeper and student.
4. **As Waste Operator & Biogas Official (Er. Rajesh Nair)**:
   - Switch to "Biogas Management" or "Animal Feed Partner".
   - Log 25 kg of spoiled food -> Notice instant high-priority alert created.
   - Click "Dispatch Truck" → "Confirm Collected" to record circular impact in kWh and animal meals.
