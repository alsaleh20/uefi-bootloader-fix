# Windows UEFI Bootloader Manual Repair Guide

A comprehensive, step-by-step guide to manually fixing and rebuilding corrupted Windows UEFI EFI bootloaders (BCD) using Command Prompt and Diskpart.

---

## Overview

This guide provides a reliable method to repair a damaged Windows UEFI Boot Configuration Data (BCD) file or EFI system partition. This issue typically occurs after system updates, partition resizing, dual-boot configurations, or unexpected shut downs that force Windows into an **Automatic Repair** loop.

---

## Prerequisites

1. Access to the Windows Recovery Environment (WinRE) via **Automatic Repair** screen or a bootable Windows Installation USB.
2. Navigate to: **Troubleshoot** > **Advanced Options** > **Command Prompt**.

---

## Step-by-Step Instructions

### Step 1: Open Diskpart and Identify the Target Disk

1. Launch the Diskpart utility:
   ```cmd
      diskpart
         ```

         2. List all available physical drives:
            ```cmd
               list disk
                  ```

                  3. Select the primary disk where Windows is installed (usually Disk 0):
                     ```cmd
                        select disk 0
                           ```

                           4. List all partitions on the selected disk:
                              ```cmd
                                 list partition
                                    ```

                                    ---

                                    ### Step 2: Locate the EFI Partition and Assign a Drive Letter

                                    1. Identify the hidden EFI System Partition. It is formatted as **FAT32** and typically ranges between **100MB and 500MB** in size.

                                    2. Select the EFI partition:
                                       ```cmd
                                          select partition X
                                             ```
                                                *(Replace `X` with the actual partition number of your FAT32 system partition)*

                                                3. Assign an unused drive letter to the partition (e.g., `V:`):
                                                   ```cmd
                                                      assign letter=V:
                                                         ```

                                                         4. Exit the Diskpart utility:
                                                            ```cmd
                                                               exit
                                                                  ```

                                                                  ---

                                                                  ### Step 3: Rebuild the Boot Configuration Data (BCD)

                                                                  1. Change directory to the EFI Boot folder:
                                                                     ```cmd
                                                                        cd /d V:\EFI\Microsoft\Boot\
                                                                           ```

                                                                           2. Attempt to repair the boot sector:
                                                                              ```cmd
                                                                                 bootrec /fixboot
                                                                                    ```

                                                                                    3. **If you encounter an "Access is Denied" error:**
                                                                                       Reformat only the assigned EFI partition (`V:`):
                                                                                          ```cmd
                                                                                             format V: /FS:FAT32
                                                                                                ```

                                                                                                4. Recreate the boot files by referencing your main Windows directory (usually `C:\Windows`):
                                                                                                   ```cmd
                                                                                                      bcdboot C:\Windows /l en-us /s V: /f ALL
                                                                                                         ```
                                                                                                            *Expected Output:* `Boot files successfully created.`

                                                                                                            ---

                                                                                                            ### Step 4: Exit and Reboot

                                                                                                            1. Exit the Command Prompt:
                                                                                                               ```cmd
                                                                                                                  exit
                                                                                                                     ```

                                                                                                                     2. Click **Continue to Windows** to reboot your machine into the operating system.

                                                                                                                     ---
                                                                                                                     ---

                                                                                                                     # دليل الإصلاح اليدوي لـ Windows UEFI Bootloader

                                                                                                                     دليل عملي وشامل لإصلاح وإعادة بناء ملفات إقلاع نظام ويندوز (BCD) المعتمدة على نظام UEFI يدويًا باستخدام موجه الأوامر وأداة Diskpart.

                                                                                                                     ---

                                                                                                                     ## نظرة عامة

                                                                                                                     يقدم هذا الدليل طريقة مجربة لإصلاح تعطل ملفات BCD أو بارتيشن EFI الخاص بإقلاع الويندوز. تحدث هذه المشكلة عادةً بسبب التحديثات، تعديل أحجام البارتيشنز، إعدادات الإقلاع المزدوج (Dual-boot)، أو الإغلاق المفاجئ للجهاز الذي يؤدي للدخول المباشر في شاشة **Automatic Repair**.

                                                                                                                     ---

                                                                                                                     ## المتطلبات الأساسية

                                                                                                                     1. الدخول إلى بيئة إصلاح الويندوز (WinRE) عبر شاشة **Automatic Repair** أو باستخدام قارورة تثبيت الويندوز (Bootable USB).
                                                                                                                     2. الانتقال إلى: **Troubleshoot** > **Advanced Options** > **Command Prompt**.

                                                                                                                     ---

                                                                                                                     ## خطوات الإصلاح التفصيلية

                                                                                                                     ### الخطوة 1: تشغيل أداة Diskpart وتحديد القرص الرئيسي

                                                                                                                     1. قم بتشغيل أداة إدارة الأقراص:
                                                                                                                        ```cmd
                                                                                                                           diskpart
                                                                                                                              ```

                                                                                                                              2. اعرض جميع الأقراص الموصلة بالجهاز:
                                                                                                                                 ```cmd
                                                                                                                                    list disk
                                                                                                                                       ```

                                                                                                                                       3. اختر القرص الرئيسي المثبت عليه نظام ويندوز (عادةً Disk 0):
                                                                                                                                          ```cmd
                                                                                                                                             select disk 0
                                                                                                                                                ```

                                                                                                                                                4. اعرض جميع التقسيمات (Partitions) الموجودة على القرص:
                                                                                                                                                   ```cmd
                                                                                                                                                      list partition
                                                                                                                                                         ```

                                                                                                                                                         ---

                                                                                                                                                         ### الخطوة 2: تحديد بارتيشن الـ EFI وتعيين حرف له

                                                                                                                                                         1. ابحث عن بارتيشن الإقلاع المخفي، ويكون بنظام ملفات **FAT32** وبحجم يتراوح عادة بين **100MB إلى 500MB**.

                                                                                                                                                         2. حدد البارتيشن المطلوب:
                                                                                                                                                            ```cmd
                                                                                                                                                               select partition X
                                                                                                                                                                  ```
                                                                                                                                                                     *(استبدل `X` برقم بارتيشن الـ FAT32 الخاص بالنظام لديك)*

                                                                                                                                                                     3. قم بتعيين حرف محرك أقراص غير مستخدم للبارتيشن (مثل `V:`):
                                                                                                                                                                        ```cmd
                                                                                                                                                                           assign letter=V:
                                                                                                                                                                              ```

                                                                                                                                                                              4. اخرج من أداة Diskpart:
                                                                                                                                                                                 ```cmd
                                                                                                                                                                                    exit
                                                                                                                                                                                       ```

                                                                                                                                                                                       ---

                                                                                                                                                                                       ### الخطوة 3: إعادة بناء ملفات الإقلاع (BCD)

                                                                                                                                                                                       1. انتقل إلى مجلد الإقلاع داخل البارتيشن المحدد:
                                                                                                                                                                                          ```cmd
                                                                                                                                                                                             cd /d V:\EFI\Microsoft\Boot\
                                                                                                                                                                                                ```

                                                                                                                                                                                                2. قم بتحديث سجل الإقلاع:
                                                                                                                                                                                                   ```cmd
                                                                                                                                                                                                      bootrec /fixboot
                                                                                                                                                                                                         ```

                                                                                                                                                                                                         3. **في حال ظهور خطأ Access is Denied:**
                                                                                                                                                                                                            قم بإعادة تهيئة بارتيشن الـ EFI المحدد فقط (`V:`):
                                                                                                                                                                                                               ```cmd
                                                                                                                                                                                                                  format V: /FS:FAT32
                                                                                                                                                                                                                     ```

                                                                                                                                                                                                                     4. أعد إنشاء ونشر ملفات النظام بالإشارة إلى مجلد الويندوز الرئيسي (`C:\Windows`):
                                                                                                                                                                                                                        ```cmd
                                                                                                                                                                                                                           bcdboot C:\Windows /l en-us /s V: /f ALL
                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                 *النتيجة المتوقعة:* ظهور رسالة `Boot files successfully created`.

                                                                                                                                                                                                                                 ---

                                                                                                                                                                                                                                 ### الخطوة 4: الخروج وإعادة التشغيل

                                                                                                                                                                                                                                 1. اخرج من موجه الأوامر:
                                                                                                                                                                                                                                    ```cmd
                                                                                                                                                                                                                                       exit
                                                                                                                                                                                                                                          ```

                                                                                                                                                                                                                                          2. اضغط على خيار **Continue to Windows** لإعادة التشغيل والإقلاع المباشر إلى النظام.