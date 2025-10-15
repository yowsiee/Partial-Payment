# Partial-Payment
# Laserfiche Payment Form Automation

This project contains JavaScript code used to manage payment entry validation, outstanding amount calculation, and UI handling within a Laserfiche electronic form.

## ✨ Features
- Calculates outstanding balance dynamically.
- Prevents overpayment.
- Marks completed payments as read-only.
- Removes delete options for finalized payments.
- Hides unnecessary payment fields depending on the selected method.
- Displays a completion message when payment is fully made.

## 🧠 Technologies
- **JavaScript (jQuery)**
- **Laserfiche Forms Platform**

## ⚙️ How it Works
The script:
1. Listens for changes in the payment amount and type.
2. Updates the outstanding balance.
3. Disables editing for completed payments.
4. Removes delete buttons to prevent modification.

## 🧩 Usage
1. Copy the script from `src/payment-form.js`.
2. Paste it into your Laserfiche form’s custom script section.
3. Adjust field IDs to match your form fields.

## 📄 License
MIT License
