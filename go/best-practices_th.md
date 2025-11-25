# Go Style Best Practices (แนวทางปฏิบัติที่ดีที่สุด)

[ภาพรวม](index) | [คู่มือหลัก](guide) | [การตัดสินใจ](decisions) | [แนวทางปฏิบัติที่ดีที่สุด](best-practices)

---

**หมายเหตุ:** เอกสารนี้ **ไม่ใช่ข้อบังคับ (Normative)** และ **ไม่ใช่มาตรฐานถาวร (Canonical)** แต่เป็นเอกสารเสริมที่ให้คำแนะนำสำหรับสถานการณ์ทั่วไป

## การตั้งชื่อ (Naming)

### ชื่อฟังก์ชันและเมธอด

- **หลีกเลี่ยงการทำซ้ำ:**
  - ไม่ต้องระบุชนิดของ input/output ถ้าไม่ชนกัน
  - ไม่ต้องซ้ำชื่อแพ็กเกจ (เช่น `yamlconfig.Parse` ดีกว่า `yamlconfig.ParseYAMLConfig`)
  - ไม่ต้องซ้ำชื่อ Receiver หรือชื่อตัวแปรพารามิเตอร์ (เช่น `Override(dest, source)` ดีกว่า `OverrideFirstWithSecond(dest, source)`)
  - ไม่ต้องระบุชนิดค่าคืนกลับ (เช่น `Transform` ดีกว่า `TransformToJSON`)
- **Convention:**
  - ฟังก์ชันที่คืนค่า: ใช้ชื่อคำนาม (เลี่ยง prefix `Get`)
  - ฟังก์ชันที่ทำงาน: ใช้ชื่อคำกริยา
  - ฟังก์ชันที่ต่างกันแค่ชนิดข้อมูล: ให้ใส่ชื่อชนิดข้อมูลต่อท้าย (เช่น `ParseInt`, `ParseInt64`)

### Test Double และ Helper Packages

- ถ้าสร้าง package แยกสำหรับ test helper ให้เติม `test` ต่อท้ายชื่อแพ็กเกจเดิม (เช่น `creditcardtest`)
- ตั้งชื่อ Stub ตามพฤติกรรม (เช่น `AlwaysCharges`, `AlwaysDeclines`)
- ตัวแปรใน Test ที่เป็น Double อาจเติม prefix เพื่อความชัดเจน (เช่น `spyCC`)

### การ Shadowing

- ระวังการใช้ `:=` ใน block ใหม่ เพราะจะเป็นการสร้างตัวแปรใหม่ที่บัง (Shadow) ตัวแปรเดิม
- ถ้าต้องการแก้ค่าเดิม ให้ใช้ `=` หรือประกาศตัวแปรด้วย `var` ก่อน

### Util Packages

- หลีกเลี่ยงชื่อแพ็กเกจกว้างๆ เช่น `util`, `helper`, `common` เพราะไม่สื่อความหมายและอาจเกิดชื่อชนกัน
- ควรตั้งชื่อตามหน้าที่ที่แพ็กเกจนั้นทำ

## ขนาดแพ็กเกจ (Package Size)

- ไม่มีกฎตายตัวเรื่อง "หนึ่ง Type หนึ่งไฟล์"
- ไฟล์ไม่ควรใหญ่เกินไปจนหาอะไรไม่เจอ หรือเล็กเกินไปจนกระจัดกระจาย
- สามารถรวมโค้ดที่เกี่ยวข้องกันไว้ในแพ็กเกจเดียวกันได้เพื่อให้ใช้งานสะดวก

## การจัดการ Error (Error Handling)

- **Error Structure:**
  - ถ้าผู้เรียกต้องการแยกแยะ error ให้ใช้ Sentinel values (ตัวแปร global), Type assertion, หรือ `errors.Is`/`errors.As`
  - อย่าเช็ค error ด้วยการเทียบ string
- **เพิ่มข้อมูลใน Error:**
  - ใช้ `%v` เพื่อเพิ่มบริบท
  - ใช้ `%w` (Wrap) เฉพาะเมื่อต้องการให้ผู้เรียกตรวจสอบ error เดิมได้ด้วย `errors.Is`/`errors.As`
  - วาง `%w` ไว้ท้ายสุดของข้อความ error
- **Logging:**
  - Log ข้อมูลที่จำเป็นสำหรับการแก้ปัญหา
  - หลีกเลี่ยงการ Log ซ้ำซ้อน (ถ้า return error แล้ว ให้ผู้เรียกตัดสินใจเองว่าจะ Log หรือไม่)
  - ใช้ `log.V` สำหรับ debug log เพื่อลด noise

## การประกาศตัวแปร (Variable Declarations)

- **Initialization:** ใช้ `:=` เมื่อมีค่าเริ่มต้นที่ไม่ใช่ zero value
- **Zero Value:** ใช้ `var` เมื่อต้องการ zero value (เช่น `var coords Point`)
- **Composite Literals:** ใช้เมื่อรู้ค่าเริ่มต้นหลายค่า (เช่น `coords := Point{X: 1, Y: 2}`)
- **Size Hints:** ใช้ `make` พร้อมระบุ capacity เฉพาะเมื่อรู้ขนาดที่แน่นอนและต้องการประสิทธิภาพจริงๆ

## รายการอาร์กิวเมนต์ของฟังก์ชัน (Function Argument Lists)

- ถ้าอาร์กิวเมนต์เยอะเกินไป:
  - **Option Struct:** สร้าง struct สำหรับรวม option ต่างๆ (เช่น `func F(options Options)`)
  - **Variadic Options:** ใช้รูปแบบ function option (เช่น `func F(opts ...Option)`) เหมาะสำหรับ API ที่ต้องการความยืดหยุ่นสูง

## การทดสอบ (Tests)

- **Test Helpers vs Assertion Helpers:**
  - **Test Helpers:** ใช้ setup/teardown ได้ (เช่น `t.Helper()`)
  - **Assertion Helpers:** ไม่แนะนำ (Not idiomatic) ให้ใช้ logic ใน `Test` function หรือใช้ library เช่น `cmp`
- **Acceptance Testing:** สร้าง test function ที่รับ interface เพื่อทดสอบ implementation ต่างๆ
- **t.Fatal vs t.Error:**
  - ใช้ `t.Fatal` เมื่อ setup ล้มเหลวและไปต่อไม่ได้
  - ใช้ `t.Error` เมื่อต้องการให้ test ทำงานต่อเพื่อดู failure อื่นๆ
- **Setup Code:**
  - Scope การ setup ให้แคบที่สุดเท่าที่จำเป็น
  - หลีกเลี่ยง `TestMain` ยกเว้นจำเป็นต้อง setup/teardown ร่วมกันจริงๆ
  - ใช้ `sync.Once` เพื่อ amortize setup cost ถ้าใช้ร่วมกันหลาย test

## การต่อสตริง (String Concatenation)

- **Simple:** ใช้ `+` สำหรับข้อความสั้นๆ
- **Formatting:** ใช้ `fmt.Sprintf` เมื่อมี format ซับซ้อน
- **Builder:** ใช้ `strings.Builder` เมื่อต่อสตริงทีละส่วนใน loop (ประสิทธิภาพดีกว่า)
- **Constant:** ใช้ Backticks (\`) สำหรับข้อความหลายบรรทัดที่เป็นค่าคงที่

## Global State

- หลีกเลี่ยง Global state ใน Library
- ให้ใช้วิธีส่ง Dependency ผ่าน constructor หรือ parameter แทน
- อนุญาตให้ใช้ package-level variable เฉพาะกรณีที่ stateless, constant, หรือมี default instance เพื่อความสะดวก (แต่ต้องมีทางเลือกให้สร้าง instance เองได้)
