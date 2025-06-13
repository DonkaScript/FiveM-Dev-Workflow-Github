# 🚀 FiveM Git Workflow & Deployment Guide

## ✅ 1. โครงสร้างหลักของการทำงาน

| สิ่งที่ใช้งาน                | บทบาท                                                        |
| ---------------------------- | ------------------------------------------------------------ |
| **GitHub**                   | ศูนย์กลาง repository และ branch ทั้งหมด                      |
| **Dev แต่ละคน (คุณ & dev2)** | clone repo มาทำงานบนเครื่องตัวเอง (local)                    |
| **VPS**                      | ใช้รันระบบจริง → ดึงโค้ดจาก branch `main` บน GitHub เท่านั้น |

---

## ✅ 2. แนวทางการทำงานของ Dev แต่ละคน

### 🌿 การทำงานที่แนะนำ:

```bash
# ก่อนเริ่มทำงาน
git checkout main
git pull origin main

# สร้าง branch แยกสำหรับงานของตัวเอง
git checkout -b feature/your-feature-name

# หลังแก้โค้ดเสร็จ
git add .
git commit -m "เพิ่มระบบ ..."
git push origin feature/your-feature-name
```

### ✅ เปิด Pull Request (PR) เพื่อรวมเข้า `main`

* เปิด PR บน GitHub เพื่อ review และ merge
* เมื่อ merge แล้ว → VPS สามารถ pull ได้

---

## ✅ 3. การทำงานบน VPS (การ Deploy จริง)

### ❌ ห้าม:

* ห้ามแก้โค้ดใน VPS โดยตรง (จะทำให้โค้ดไม่ sync กับ GitHub และอาจพัง)

### ✅ ให้ทำสิ่งนี้เท่านั้น:

```bash
cd /path/to/your/repo
git pull origin main
```

### 🔁 ควร pull เมื่อ:

* มีการ merge pull request เข้าสู่ `main` แล้ว
* หรือมีระบบ webhook / CI/CD สั่ง pull ให้อัตโนมัติ

---

## ✅ 4. ใครควรเป็นคน pull โค้ดใน VPS?

### 🧭 คำตอบหลัก: **"ใคร merge PR เข้าสู่ `main` → คนนั้นต้องเข้า VPS และ pull"**

### 🔄 กรณีอื่น ๆ ที่ทำได้:

* ตกลงกันว่า dev คนไหนจะเป็นเวร pull
* แจ้งใน Discord หรือ LINE เมื่อ merge เสร็จ
* ใช้ระบบ webhook / GitHub Actions ทำให้ VPS pull อัตโนมัติ

---

## ✅ 5. Workflow ภาพรวม

```text
[dev1: feature/amb-checker]       [dev2: bugfix/x]
           ↓                            ↓
   [Push to GitHub → Pull Request → main branch]
                          ↓
               [VPS: git pull origin main]
```

---

## ✅ 6. หมายเหตุและคำแนะนำเพิ่มเติม

| หัวข้อ     | แนะนำ                                                             |
| ---------- | ----------------------------------------------------------------- |
| การแก้โค้ด | ให้ทำบน local แล้ว push ขึ้น GitHub เท่านั้น                      |
| การแยกงาน  | ให้ใช้ branch ชื่อเฉพาะ เช่น `feature/xxx`, `bugfix/yyy`          |
| การรวมโค้ด | ใช้ Pull Request (PR) เพื่อ review และ merge                      |
| VPS        | เป็นเพียงฝั่ง "รันโค้ด" จาก branch main เท่านั้น ไม่ควรแก้อะไรเอง |
| การสื่อสาร | แจ้งในช่องทางทีม (LINE/Discord) เมื่อ merge เสร็จแล้ว pull ได้    |

---

## ✅ 7. ทางเลือก: ระบบ Auto Pull (CI/CD)

หากไม่อยากเข้า VPS manual ทุกครั้ง:

* ใช้ GitHub Actions + SSH deploy
* หรือใช้ GitHub Webhook รัน script pull บน VPS อัตโนมัติ

> ตัวอย่าง: `appleboy/ssh-action` หรือ Webhook Listener

---

## ✅ สรุปสุดท้าย

| คน          | ทำอะไร                                           |
| ----------- | ------------------------------------------------ |
| Dev แต่ละคน | clone → แยก branch → push → PR เข้า main         |
| GitHub      | ศูนย์กลางควบคุมเวอร์ชันทั้งหมด                   |
| VPS         | ดึงจาก main เท่านั้น เพื่อใช้งาน production จริง |

> ✅ ทุกคนควรทำงานผ่าน Git และ GitHub เท่านั้น เพื่อความปลอดภัยและไม่ conflict
