## Terraform Command Line Interface 




> ### Overview
\
**Terraform CLI (Command-Line Interface)** เป็นเครื่องมือที่ใช้สำหรับการจัดการโครงสร้างพื้นฐานแบบ Infrastructure as Code (IaC) จะทำหน้าที่ในการ building, changing, and versioning infrastructure บน cloud providers ต่างๆ เช่น AWS, Azure, Google Cloud โดยเราสามารถ provisioning infrastructure ผ่านวิธีการเขียน coding ด้วยภาษา Terraform Configuration Language (HCL)

<br>

### คำสั่งที่สำคัญที่ใช้บ่อยใน Terraform Command Line

| คำสั่ง                |	 คำอธิบาย                                      |
| ------------------- |	 ------------------------------------------  |
| terraform init	    |  เตรียม Terraform และดาวน์โหลด plugins ที่จำเป็น ตาม Configuration  | 
| terraform plan	    |  พรีวิวการเปลี่ยนแปลงก่อน Deploy|
| terraform apply	    |  ใช้เพื่อ Deploy ทรัพยากรจริง|
| terraform destroy  	|  ลรัพยากรทั้งหมดที่สร้างโดย Terraform|
| terraform show	    |  ดูรายละเอียดของทรัพยากรที่ Terraform จัดการอยู่|
| terraform state	    |  ใช้จการ State File เช่น ดู, ลบ, หรือแก้ไขสถานะ|
| terraform validate	|  วอบความถูกต้องของโค้ด Terraform|
| terraform fmt	      |  จัดบบโค้ด Terraform ให้สวยงาม|
| terraform workspace  | จัดการช่วยให้เราสามารถแยกสภาพแวดล้อมการทำงานออกจากกัน |

<br>

## Terraform `init` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/init)

`terraform init` เป็นคำสั่งแรกที่ต้องใช้ก่อนเริ่มใช้งาน Terraform ในโปรเจกต์ใหม่ หรือเมื่อมีการเปลี่ยนแปลงโครงสร้างไฟล์หลัก (.tf files)
<br>

### 🛠️ การใช้งาน `terraform init`

```sh
terraform init [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `-input=true` | Terraform จะขอข้อมูลจากผู้ใช้หากจำเป็น ถ้ากำหนดเป็น `false` และมีการต้องป้อนข้อมูล จะเกิดข้อผิดพลาด |
| `-lock=false` | ปิดการล็อกไฟล์ State ในระหว่างการทำงาน เช่น การดาวน์โหลด Providers หรือ Backend |
| `-lock-timeout=<duration>` | กำหนดเวลาที่ Terraform จะรอการล็อก State (ค่าเริ่มต้นคือ `0s` หมายถึงไม่รอ) |
| `-no-color` | ปิดการใช้สีในผลลัพธ์ของคำสั่ง (เหมาะสำหรับการใช้งานในระบบที่ไม่รองรับ ANSI color codes) |
| `-upgrade` | อัปเกรด Plugins และ Modules ที่ใช้เป็นเวอร์ชันล่าสุด |


<br>

## Terraform `plan` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/plan)

`terraform plan` เป็นคำสั่งที่ใช้สำหรับ แสดงการเปลี่ยนแปลงของโครงสร้างพื้นฐาน โดยไม่ทำการเปลี่ยนแปลงจริง เพื่อให้ผู้ใช้สามารถตรวจสอบว่าคำสั่ง terraform apply จะทำอะไรบ้าง


### 🛠️ การใช้งาน `terraform plan`

```sh
terraform plan [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `terraform plan -out=<file>` | บันทึกผลลัพธ์ของแผนไปยังไฟล์ `.tfplan` |
| `terraform plan -destroy` | ตรวจสอบการลบทุกทรัพยากร |

<br>

## Terraform `apply` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/apply)

`terraform apply` เป็นคำสั่งที่ใช้ในการดำเนินการเปลี่ยนแปลงโครงสร้างพื้นฐานตามแผนที่กำหนดไว้ใน Terraform Configuration (`.tf` files) โดย Terraform จะสร้าง, อัปเดต หรือ ลบทรัพยากรที่ระบุใน State File

### 🛠️ การใช้งาน `terraform apply`

```sh
terraform apply [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `-auto-approve` | ใช้เพื่อข้ามการขออนุมัติ (ไม่ต้องพิมพ์ `yes`) เหมาะกับ CI/CD |
| `<plan-file>` | ใช้แผนที่สร้างจาก `terraform plan -out=<file>` เพื่อลดความเสี่ยงในการเปลี่ยนแปลง |
| `-var="key=value"` | กำหนดค่าตัวแปรแบบกำหนดเองขณะ Apply |
| `-var-file=<file>` | ใช้ไฟล์ `.tfvars` สำหรับกำหนดค่าตัวแปร |

<br>

## Terraform `destroy` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/destroy)

`terraform destroy` เป็นคำสั่งที่ใช้สำหรับลบทรัพยากรทั้งหมดที่ถูกสร้างขึ้นโดย Terraform ตามที่กำหนดไว้ใน State File (`terraform.tfstate`) 


### 🛠️ การใช้งาน `terraform destroy`

```sh
terraform destroy [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `-auto-approve` | ลบทรัพยากรโดยไม่ต้องขออนุมัติ (ข้ามขั้นตอนการพิมพ์ `yes`) |
| `-target=<resource>` | ลบเฉพาะทรัพยากรที่ระบุ เช่น `-target=aws_instance.example` |
| `-var="key=value"` | ใช้ค่าตัวแปรที่กำหนดเองขณะทำลายทรัพยากร |
| `-var-file=<file>` | ใช้ไฟล์ `.tfvars` สำหรับกำหนดค่าตัวแปร |
<br>

## Terraform `show` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/show)

`terraform show` เป็นคำสั่งที่ใช้สำหรับแสดงรายละเอียดของ **Terraform State File (`terraform.tfstate`)** หรือแสดงเนื้อหาของ **Terraform Plan File** ที่ถูกสร้างจาก `terraform plan -out=<file>`

### 🛠️ การใช้งาน `terraform show`

```sh
terraform show [options] [file]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| ` <plan-file>` | แสดงรายละเอียดของแผน (`.tfplan`) ที่ถูกบันทึกจาก `terraform plan -out=<file>` |
| ` -json` | แสดงผลลัพธ์ในรูปแบบ JSON ซึ่งเหมาะสำหรับการใช้งานร่วมกับระบบอัตโนมัติ (Automation) |

## Terraform `state` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/state)

`terraform state` เป็นชุดคำสั่งที่ใช้สำหรับจัดการ **Terraform State File (`terraform.tfstate`)** ซึ่งช่วยให้สามารถดู, แก้ไข, ย้าย หรือแม้แต่ลบทรัพยากรใน State ได้โดยไม่ต้องเปลี่ยนโค้ด Terraform Configuration (`.tf` files)


### 🛠️ การใช้งาน `terraform state`

```sh
terraform state <subcommand> [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `list` | แสดงรายการทรัพยากรทั้งหมดที่อยู่ใน State File |
| `show <resource>` | แสดงรายละเอียดของทรัพยากรที่ระบุ เช่น `terraform state show aws_instance.example` |
| `mv <source> <destination>` | ย้ายทรัพยากรใน State เช่น `terraform state mv aws_instance.old aws_instance.new` |
| `pull` | ดาวน์โหลด State ปัจจุบันเป็น JSON (เหมาะสำหรับการสำรองข้อมูล) |
| `push` | อัปโหลดไฟล์ State ไปยัง Remote Backend (ต้องเปิดใช้ Remote Backend) |
| `rm <resource>` | ลบทรัพยากรออกจาก State (แต่ไม่ลบจริงจากระบบ) |
<br>

## Terraform `validate` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/validate)

`terraform validate` เป็นคำสั่งที่ใช้สำหรับ **ตรวจสอบความถูกต้องของไฟล์ Terraform Configuration (`.tf` files)** โดยไม่ต้องใช้ Provider หรือ Backend เพื่อให้แน่ใจว่าโค้ดไม่มีข้อผิดพลาดทางไวยากรณ์หรือโครงสร้าง

### 🛠️ การใช้งาน `terraform validate`

```sh
terraform validate [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `-json` | ส่งออกผลลัพธ์เป็น JSON (เหมาะสำหรับการ Integrate กับระบบอัตโนมัติ) |

<br>

## Terraform `fmt` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/fmt)

`terraform fmt` เป็นคำสั่งที่ใช้สำหรับ **จัดรูปแบบโค้ด Terraform (`.tf` และ `.tfvars` files) ให้เป็นไปตามมาตรฐานของ Terraform** เพื่อให้โค้ดอ่านง่ายและเป็นระเบียบมากขึ้น


### 🛠️ การใช้งาน `terraform fmt`

```sh
terraform fmt [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `-recursive` | จัดรูปแบบไฟล์ `.tf` ทั้งหมดในไดเรกทอรีย่อยด้วย |
| `-diff` | แสดงความแตกต่างระหว่างโค้ดเดิมและโค้ดที่จัดรูปแบบใหม่ โดยไม่เปลี่ยนแปลงไฟล์ |
| `-check` | ตรวจสอบว่าไฟล์ `.tf` เป็นไปตามมาตรฐานหรือไม่ (หากไม่ตรง จะคืนค่า error) |

<br>

## Terraform `workspace` Command [#Reference](https://developer.hashicorp.com/terraform/cli/commands/workspace)

`terraform workspace` เป็นคำสั่งที่ใช้สำหรับ **จัดการ Workspaces** ใน Terraform ซึ่งช่วยให้สามารถแยก State File สำหรับแต่ละ Environment (เช่น `dev`, `staging`, `prod`) โดยใช้โค้ดเดียวกัน

### 🛠️ การใช้งาน `terraform workspace`

```sh
terraform workspace <subcommand> [options]
```
<br>

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `list` | แสดงรายชื่อ Workspaces ที่มีอยู่ |
| `show` | แสดง Workspace ปัจจุบันที่กำลังใช้งาน |
| `new <name>` | สร้าง Workspace ใหม่ |
| `<name>` | เปลี่ยนไปใช้ Workspace ที่ระบุ |
| `delete <name>` | ลบ Workspace ที่กำหนด (ต้องไม่ใช้งานอยู่) |
