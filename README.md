# a11yFlutter
# Flutter Accessibility Guidance

## Description
This repository provides a comprehensive set of accessibility guidelines for Flutter developers, designed to help DevSet IA generate code that meets the European Accessibility Act (EAA) requirements effective from June 28, 2025.

The goal is to ensure that all generated Flutter code is compliant with the Référentiel d’Audit d’Accessibilité Mobile (RAAM) and follows inclusive design principles for users with disabilities.

---

## File Overview
**File:** `aa11yFlutter.md`

Disclaimer : This is designed as a help for developpers. However, you still need to do manual test (preferably with people with disabilities and digital accessibility experts) and this is not exhaustive content. 

This file contains:
- Practical examples of accessible Flutter code (buttons, images, forms, lists, alerts, etc.)  
- Guidelines covering cognitive, visual, keyboard, and assistive technology accessibility  
- Implementation notes explaining platform differences between Android and iOS  
 

---

## Usage

1. Place this file in your DevSet IA or Flutter project repository.  
2. Ensure it is applied to all Dart files via the `applyTo: "**/*.dart"` directive in the header.  
3. Use it as a reference when generating or reviewing Flutter code for accessibility compliance.  
4. Always validate accessibility using :
   - Accessibility Inspector (iOS) ; 
   - Accessibility Scanner (Android) ; 
   - And most importantly manual screen reader testing (TalkBack, VoiceOver) on last OS versions. 

---

## Author
**Tiffany Voguin**  
Digital accessibility consultant

---

## License
This guidance is open for reuse and adaptation for educational and professional accessibility compliance purposes. Please credit the author where applicable.
