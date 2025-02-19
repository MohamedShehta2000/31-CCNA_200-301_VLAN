## VLAN (Virtual LAN)

**VLAN** هي تقسيم منطقي على مستوى الطبقة الثانية في الشبكة، حيث تعتبر كل VLAN مجال بث (Broadcast Domain) منفصل، وعادةً ما يكون لها شبكة IP خاصة بها. تُعزل VLANs عن بعضها البعض، ولا يمكن تبادل البيانات بينها إلا من خلال جهاز راوتر.

### أنواع التراسل في الشبكات
- **Unicast**: إرسال من جهاز إلى جهاز آخر محدد.
- **Broadcast**: إرسال من جهاز إلى جميع الأجهزة الأخرى، مما يسبب بطء في الشبكة؛ لهذا نقوم بإنشاء VLANs لتقسيم الشبكة.

### فوائد VLANs
1. **الأمان**: فصل الشبكات بحيث يكون كل قسم منعزل بذاته.
2. **تقليل البث**: التغلب على مشاكل البث العالي (Broadcast) في الشبكة.
3. **سهولة الإدارة**: متابعة وإدارة كل قسم أو شبكة بشكل منفصل.

### ملاحظات مهمة
- **VLAN 1**: هي VLAN الأساسية (تأتي بشكل افتراضي على جميع أجهزة الـSwitch)، ولا يمكن حذفها أو إعادة تسميتها.
- **VLAN ID**: لكل VLAN رقم معرف (ID) يتمثل في 12 بت، مما يتيح أرقاماً بين 0 و4096.
- **أنواع VLAN**:
  - **Normal Range** (VLAN 1-1005): غالباً ما نستخدم VLANs بين 2 و1001، أما VLANs 1002-1005 فمخصصة لتطبيقات محددة مثل Token Ring.
  - **Extended Range** (VLAN 1006-4094): تُستخدم عادةً لمزودي الخدمة (Service Providers).
  - **VLAN 0 و4095**: تكون معطّلة (Disable) ولكن يمكن تفعيلها وتخصيصها للصوت.

### خطوات إعداد VLANs

1. **إنشاء VLANs**:
   ```plaintext
   vlan 2
   name HR
   vlan 3
   name Sales
   vlan 4
   name Accounting
   ```

2. **توزيع المنافذ**:
   ```plaintext
   int f0/1
   switchport access vlan 2
   exit
   int f0/2
   switchport access vlan 2
   exit
   int range f0/3-4
   switchport access vlan 3
   exit
   int range f0/5-6
   switchport access vlan 4
   ```

3. **ربط Switch جديد بـVLAN معينة**:
   - في حال إضافة **Switch** جديد وربط أجهزة به تحتاج لتحديد VLAN معينة.
   - إذا واجهت مشكلة في الاتصال عبر **ping**، ستكون المشكلة في الرابط بين **Switches**.

4. **أنماط المنافذ على Switch**:
   - **Access Mode**: يُستخدم للاتصال بجهاز حاسوب، ويسمح بمرور VLAN واحدة فقط.
   - **Trunk Mode**: يُستخدم للاتصال بين **Switches**، ويسمح بمرور كل الـVLANs.

5. **إعداد Trunk بين Switches**:
   ```plaintext
   ena
   config t
   int f0/24
   switchport mode trunk
   ```

### التوجيه بين VLANs
- لتمكين التواصل بين VLANs، يمكن استخدام **Switch من الطبقة الثالثة (Layer 3 Switch)**.
- يُفضل تنفيذ **Subnetting** أولاً لتحديد نطاقات IP لكل VLAN قبل تفعيل الاتصال المتبادل.
