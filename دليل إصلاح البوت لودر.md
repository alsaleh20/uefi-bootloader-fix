# Windows EFI Bootloader Manual Repair Guide using Diskpart
> دليل الإصلاح اليدوي لإقلاع ويندوز باستخدام Diskpart

---

## English Version

A comprehensive step-by-step guide detailing the necessary commands and actions to rebuild and repair Windows (Windows 10/11) boot files (BCD) in a UEFI environment when the system crashes or boots directly into the Automatic Repair screen.

---

### Prerequisites

1. Access the Automatic Repair screen.
2. Navigate to the Command Prompt following this path:
   - Troubleshoot
   - Advanced Options
   - Command Prompt

---

### Manual Repair Steps

#### Step 1: Launch Diskpart and Select the Main Disk

Open the disk management tool and display the list to identify the OS drive:

```cmd
diskpart
list disk
select disk 0
list partition
```

Note: Make sure to select the correct disk number (usually Disk 0).

---

#### Step 2: Identify the EFI Partition and Assign a Drive Letter

Look through the partitions list for the hidden boot partition, identified by:
- File System: FAT32
- Size: Between 100MB and 500MB

Execute the following commands to assign a temporary drive letter (e.g., V:):

```cmd
select partition X
assign letter=V:
exit
```

Warning: Replace X with your actual FAT32 partition number.

---

#### Step 3: Rebuild Boot Files (BCD)

Navigate to the boot folder and rebuild the boot files:

```cmd
cd /d V:\EFI\Microsoft\Boot\
bootrec /fixboot
```

##### In case of an "Access is denied" error:
Format only the designated EFI boot partition (ensure you are acting on the assigned letter V:):

```cmd
format V: /FS:FAT32
```

Then rebuild the system files pointing to your main Windows directory:

```cmd
bcdboot C:\Windows /l en-us /s V: /f ALL
```

Success Indicator: You should see the message: "Boot files successfully created".

---

#### Step 4: Exit and Reboot

Exit the command prompt and restart your system:

```cmd
exit
```

Click Continue to Windows to boot directly into your operating system.

---
---

## النسخة العربية

دليل شامل ومفصل يتضمن الأوامر والخطوات اللازمة لإعادة بناء وإصلاح ملفات الإقلاع (BCD) لنظام ويندوز (Windows 10/11) في بيئة UEFI عند تعطل النظام أو توجهه المباشر إلى شاشة Automatic Repair.

---

### المتطلبات الأساسية

1. الوصول إلى شاشة Automatic Repair (الإصلاح التلقائي).
2. الانتقال إلى موجه الأوامر بالترتيب التالي:
   - Troubleshoot (استكشاف الأخطاء وإصلاحها)
   - Advanced Options (خيارات متقدمة)
   - Command Prompt (موجه الأوامر)

---

### خطوات الإصلاح اليدوي

#### الخطوة 1: تشغيل أداة Diskpart وتحديد القرص الرئيسي

افتح أداة إدارة الأقراص واعرض القائمة لتحديد القرص المثبت عليه نظام التشغيل:

```cmd
diskpart
list disk
select disk 0
list partition
```

ملاحظة: اختر رقم القرص الصحيح (عادة ما يكون Disk 0).

---

#### الخطوة 2: تحديد بارتيشن الـ EFI وتعيين حرف محرك أقراص

ابحث في قائمة التقسيمات (Partitions) عن البارتيشن المخفي الخاص بالإقلاع، والذي يتميز بالخصائص التالية:
- نظام الملفات: FAT32
- الحجم: بين 100MB إلى 500MB

نفذ الأوامر التالية لإعطائه حرفاً موقتاً (مثلاً V:):

```cmd
select partition X
assign letter=V:
exit
```

تنبيه: استبدل X برقم بارتيشن الـ FAT32 الظاهر لديك.

---

#### الخطوة 3: إعادة بناء ملفات الإقلاع (BCD)

انقل مسار العمل إلى مجلد الإقلاع واكتب ملفات الإقلاع الجديدة:

```cmd
cd /d V:\EFI\Microsoft\Boot\
bootrec /fixboot
```

##### في حال ظهور خطأ (Access is denied):
قم بإعادة تهيئة بارتيشن الـ EFI الخاص بالإقلاع فقط (تأكد من اختيار الحرف V: المعين سابقاً):

```cmd
format V: /FS:FAT32
```

ثم أعد بناء ملفات النظام بالإشارة إلى مجلد الويندوز الرئيسي:

```cmd
bcdboot C:\Windows /l en-us /s V: /f ALL
```

علامة النجاح: ستظهر لك الرسالة التالية: Boot files successfully created.

---

#### الخطوة 4: الخروج وإعادة التشغيل

اخرج من موجه الأوامر وأعد تشغيل الجهاز:

```cmd
exit
```

اضغط على Continue to Windows للإقلاع المباشر لنظام التشغيل.

---

## License & Notes
- Designed specifically for UEFI/GPT systems.
- Created for documentation and reference during bootloader recovery without data loss.