# Go Style Decisions (การตัดสินใจเรื่องสไตล์)

[ภาพรวม](index_th) | [คู่มือหลัก](guide_th) | [การตัดสินใจ](decisions_th) | [แนวทางปฏิบัติที่ดีที่สุด](best-practices_th)

---

**หมายเหตุ:** เอกสารนี้เป็น **เกณฑ์ปฏิบัติ (Normative)** แต่ **ไม่ใช่มาตรฐานถาวร (Canonical)** หากมีข้อขัดแย้งกับ [คู่มือหลัก (Guide)](guide) ให้ยึดตามคู่มือหลักเป็นสำคัญ

## เกี่ยวกับ

เอกสารนี้ประกอบด้วยการตัดสินใจด้านสไตล์ที่มุ่งสร้างความเป็นอันหนึ่งอันเดียวกันและให้คำแนะนำที่เป็นมาตรฐาน คำอธิบาย และตัวอย่างสำหรับคำแนะนำที่ได้รับจากผู้ให้คำปรึกษาด้านความสามารถในการอ่านโค้ด Go (Go readability mentors)

เอกสารนี้ **ไม่ได้ครอบคลุมทุกเรื่อง** และจะเพิ่มพูนขึ้นเรื่อย ๆ เมื่อเวลาผ่านไป ในกรณีที่ คู่มือสไตล์หลัก (the core style guide) ขัดแย้งกับคำแนะนำที่ระบุไว้ ณ ที่นี้ ให้ถือว่า คู่มือสไตล์มีความสำคัญเหนือกว่า และเอกสารนี้ควรได้รับการปรับปรุงแก้ไขให้สอดคล้องกัน

โปรดดูที่ [ภาพรวม](https://google.github.io/styleguide/go#about) สำหรับชุดเอกสาร Go Style ฉบับเต็ม

หัวข้อต่อไปนี้ได้ถูกย้ายจาก style decisions ไปยังส่วน guide:

- **MixedCaps**: see [guide#mixed-caps](guide#mixed-caps)
  <a id="mixed-caps"></a>

- **Formatting**: see [guide#formatting](guide#formatting)
  <a id="formatting"></a>

- **Line Length**: see [guide#line-length](guide#line-length)
  <a id="line-length"></a>

<a id="naming"></a>

## การตั้งชื่อ (Naming)

### การใช้เครื่องหมายขีดล่าง (Underscores)

โดยทั่วไปชื่อใน Go **ไม่ควร** มีเครื่องหมายขีดล่าง (`_`) ยกเว้น 3 กรณี:

1.  ชื่อแพ็กเกจที่ถูก import โดย generated code เท่านั้น
2.  ชื่อฟังก์ชัน Test, Benchmark และ Example ในไฟล์ `*_test.go`
3.  Library ระดับล่างที่ทำงานร่วมกับ OS หรือ cgo (เช่น `syscall`)

### ชื่อแพ็กเกจ (Package names)

- ต้องกระชับ ใช้ตัวพิมพ์เล็กทั้งหมด และไม่มีขีดล่าง (เช่น `tabwriter` ไม่ใช่ `TabWriter` หรือ `tab_writer`)
- หลีกเลี่ยงชื่อที่ซ้ำกับตัวแปรทั่วไป (เช่น `usercount` ดีกว่า `count`)
- หลีกเลี่ยงชื่อที่ไม่สื่อความหมาย เช่น `util`, `common`, `helper`

### ชื่อ Receiver (Receiver names)

- ควรใช้ตัวอักษรเพียง หนึ่งหรือสองตัวอักษร เท่านั้น
- ควรใช้ตัวอักษรที่เป็น คำย่อ หรือตัวย่อของชื่อ Type นั้น ๆ เอง (เช่น `func (c *Client)` ไม่ใช่ `func (cl *Client)`)
- ใช้ชื่อเดิมอย่างสม่ำเสมอตลอดทั้ง สำหรับ Receiver ทุกตัวของ Type เดียวกันนั้น ๆ (เช่น (u User) ก็ควรเป็นแบบนี้ตลอด)

| Long Name                   | Better Name               |
| --------------------------- | ------------------------- |
| `func (tray Tray)`          | `func (t Tray)`           |
| `func (info *ResearchInfo)` | `func (ri *ResearchInfo)` |
| `func (this *ReportWriter)` | `func (w *ReportWriter)`  |
| `func (self *Scanner)`      | `func (s *Scanner)`       |

[Receiver]: https://golang.org/ref/spec#Method_declarations

### ชื่อค่าคงที่ (Constant names)

- ต้องใช้รูปแบบ [MixedCaps] เหมือนกับชื่ออื่นๆ ทั้งหมดในภาษา Go.(เช่น `MaxPacketSize` ไม่ใช่ `MAX_PACKET_SIZE`)
- [Exported] ค่าคงที่เริ่มต้นด้วยตัวพิมพ์ใหญ่ ในขณะที่ค่าคงที่ที่ไม่ได้ส่งออกจะเริ่มต้นด้วยตัวพิมพ์เล็ก
- ค่าคงที่ที่ [Exported] (Exported) จะเริ่มต้นด้วยตัวพิมพ์ใหญ่ (Uppercase) ในขณะที่ค่าคงที่ที่ ไม่ได้ส่งออก (Unexported) จะเริ่มต้นด้วยตัวพิมพ์เล็ก (Lowercase)
  หลักการนี้ใช้ได้แม้ว่ามันจะขัดกับธรรมเนียมปฏิบัติในภาษาอื่น ๆ ก็ตาม ชื่อค่าคงที่ไม่ควรเป็นเพียงการดัดแปลงมาจากค่าของมัน แต่ควรอธิบายว่าค่านั้นสื่อถึงอะไรแทน
  อธิบายว่าค่านั้นสื่อถึงอะไร

```go
// Good:
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)
```

[MixedCaps]: guide#mixed-caps
[Exported]: https://tour.golang.org/basics/3

อย่าใช้ชื่อค่าคงที่ที่ไม่ใช่ MixedCaps หรือค่าคงที่ที่มีคำนำหน้า `K`

```go
// Bad:
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```

ตั้งชื่อค่าคงที่ตามบทบาท ไม่ใช่ตามค่า หากค่าคงที่ไม่มีบทบาทนอกเหนือจากค่าของมัน ก็ไม่จำเป็นต้องนิยามให้เป็นค่าคงที่

```go
// Bad:
const Twelve = 12

const (
    UserNameColumn = "username"
    GroupColumn    = "group"
)
```

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="initialisms"></a>

<span style="color: red; font-size: 1.5em;">
 -----------check point----------
</span>

### ตัวย่อ (Initialisms)

- คำที่เป็นตัวย่อ (เช่น URL, ID) ต้องใช้ตัวพิมพ์เดียวกันทั้งคำ (เช่น `serveHTTP`, `XMLAPI`, `urlPony`)
- ยกเว้นกรณีที่ต้องเปลี่ยนตัวแรกเพื่อการ export/unexport (เช่น `xmlAPI` ถ้า unexport แต่ `XMLAPI` ถ้า export)

### Getters

- ไม่ต้องมีคำว่า `Get` นำหน้า (เช่น `obj.Owner()` ไม่ใช่ `obj.GetOwner()`)
- ยกเว้นถ้าเป็นการดึงข้อมูลที่ซับซ้อนหรือมีการคำนวณ อาจใช้ `Compute` หรือ `Fetch` แทน

### ชื่อตัวแปร (Variable names)

- ความยาวแปรผันตามขอบเขต (Scope): Scope เล็กใช้ชื่อสั้น Scope ใหญ่ใช้ชื่อยาวและชัดเจนขึ้น
- หลีกเลี่ยงการใส่ชนิดตัวแปรในชื่อ (เช่น `users` ดีกว่า `userSlice`)
- **ตัวแปรตัวอักษรเดียว:** เหมาะสำหรับ Loop index (`i`), พิกัด (`x`, `y`), หรือชื่อ Receiver ที่ชัดเจน

### การทำซ้ำ (Repetition)

- **Package vs Exported Symbol:** อย่าตั้งชื่อซ้ำกับแพ็กเกจ (เช่น `widget.New` ดีกว่า `widget.NewWidget`)
- **Context:** อย่าใส่ข้อมูลที่รู้อยู่แล้วจากบริบท (เช่น ในเมธอด `User.Name()` ไม่ต้องเป็น `User.UserName()`)

---

## คอมเมนต์ (Commentary)

- **Doc Comments:** ชื่อที่ Export ต้องมี Doc comment เสมอ โดยเริ่มด้วยชื่อของสิ่งนั้นและเป็นประโยคที่สมบูรณ์
  - เช่น `// Request represents a request to run a command.`
- **Package Comments:** ต้องอยู่เหนือ `package` clause โดยไม่มีบรรทัดว่างคั่น

---

## การนำเข้า (Imports)

### การเปลี่ยนชื่อ (Import renaming)

- เปลี่ยนชื่อเฉพาะเมื่อจำเป็น (เช่น ชื่อซ้ำ) หรือเพื่อความอ่านง่าย
- Generated protobuf **ต้อง** เปลี่ยนชื่อให้ลงท้ายด้วย `pb`
- ถ้าชื่อแพ็กเกจไม่สื่อความหมาย (เช่น `v1`) สามารถเปลี่ยนชื่อได้

### การจัดกลุ่ม (Import grouping)

เรียงลำดับดังนี้:

1.  Standard library
2.  Project และ Third-party packages
3.  Protocol Buffer imports
4.  Side-effects imports (`_`)

### Import "dot" (`import .`)

- **ห้ามใช้** ใน Google codebase เพราะทำให้สับสนที่มาของฟังก์ชัน

---

## ข้อผิดพลาด (Errors)

### การคืนค่า Error (Returning errors)

- ใช้ `error` เป็นค่าคืนกลับตัวสุดท้าย
- คืนค่า `nil` เมื่อไม่มี error
- **ห้าม** คืนค่า Concrete error type (เช่น `*os.PathError`) เพราะอาจเกิดปัญหา `nil` interface

### ข้อความ Error (Error strings)

- ไม่ควรขึ้นต้นด้วยตัวใหญ่และไม่ควรมีเครื่องหมายจบประโยค (เพราะมักถูกนำไปต่อกับข้อความอื่น)
  - เช่น `fmt.Errorf("something bad happened")`

### การจัดการ Error (Handle errors)

- อย่าทิ้ง error ด้วย `_`
- จัดการทันที, คืนค่ากลับไป, หรือ `log.Fatal`/`panic` ในกรณีร้ายแรง
- ใช้ Indent เพื่อแยกส่วน Error handling ออกจากโค้ดปกติ (Error อยู่ใน `if`, โค้ดปกติอยู่นอก `else`)

### In-band errors

- หลีกเลี่ยงการคืนค่าพิเศษ (เช่น -1 หรือ null) เพื่อบอก error ให้คืนค่า `error` หรือ `bool` เพิ่มอีกตัวแทน
  - เช่น `func Lookup(key string) (string, bool)`

---

## ภาษา (Language)

### การจัดรูปแบบ Literal (Literal formatting)

- **Field names:** Struct literal ที่ใช้ type จากแพ็กเกจอื่น **ต้อง** ระบุชื่อ field เสมอ
- **Zero-value:** ละ field ที่เป็น zero-value ได้ถ้าไม่ทำให้สับสน
- **Cuddled braces:** การเขียน `}, {` ในบรรทัดเดียว ทำได้เฉพาะเมื่อข้างในเป็น literal หรือ proto builder และ indent ตรงกัน

### Nil slices

- `nil` slice และ empty slice (`[]int{}`) มีพฤติกรรมเหมือนกันในคนส่วนใหญ่
- ชอบใช้ `var s []int` (เป็น nil) มากกว่า `s := []int{}`
- อย่าสร้าง API ที่แยกความแตกต่างระหว่าง nil กับ empty slice

### Indentation confusion

- หลีกเลี่ยงการตัดบรรทัดที่ทำให้ indent ของเงื่อนไขตรงกับ indent ของ block code ข้างใน

### Function formatting

- Signature ของฟังก์ชันควรอยู่ในบรรทัดเดียวถ้าทำได้
- ถ้าต้องตัดบรรทัด argument ให้ตัดตามกลุ่มความหมาย ไม่ใช่แค่เพราะความยาว

### Conditionals & Loops

- `if` ไม่ควรตัดบรรทัด
- `switch` และ `case` ควรอยู่บรรทัดเดียว
- **Yoda style:** เลี่ยงการเขียน `if "foo" == result` ให้เขียน `if result == "foo"`

### การคัดลอก (Copying)

- ระวังการ copy struct ที่มี `sync.Mutex` หรือ `bytes.Buffer` (เพราะอาจเกิด bugs)
- ถ้า struct ไม่ควรถูก copy ให้ใช้ pointer receiver

### Panic

- อย่าใช้ `panic` สำหรับ error ปกติ ให้ใช้ `error`
- ใช้ `panic` หรือ `log.Fatal` เฉพาะกับข้อผิดพลาดที่ "เป็นไปไม่ได้" หรือตอนเริ่มโปรแกรม (init)

### Must functions

- ฟังก์ชัน `MustXYZ` (เช่น `MustParse`) ใช้สำหรับช่วย setup ที่จะ panic ถ้าทำงานไม่สำเร็จ
- ควรใช้เฉพาะตอนเริ่มโปรแกรม (init) หรือใน Tests

### Interfaces

- Interface ควรนิยามในแพ็กเกจที่ **ใช้** (Consumer) ไม่ใช่แพ็กเกจที่ทำ (Implementer)
- รับ Interface คืน Concrete type

### Generics

- ใช้อย่างระมัดระวัง อย่าใช้แค่เพราะ "ทำได้"
- ถ้า Code ทำงานกับ Type เดียว ไม่ต้องใช้ Generics

### Pass Values vs Pointers

- อย่าส่ง Pointer เพียงเพื่อประหยัด Memory เล็กน้อย (เช่น `*string`, `*int`)
- Proto buffer ควรส่งเป็น Pointer เสมอ

### Receiver Type

- ถ้า method ต้องแก้ค่าใน receiver -> ใช้ **Pointer**
- ถ้า struct ห้าม copy (เช่นมี Mutex) -> ใช้ **Pointer**
- ถ้าเป็น slice, map, channel, function -> ใช้ **Value**
- ถ้าเป็น simple basic type (int, string) -> ใช้ **Value**
- ถ้าไม่แน่ใจ -> ใช้ **Pointer**

---

## ไลบรารีทั่วไป (Common libraries)

- **Flags:** ชื่อ Flag ใน command line ใช้ `snake_case` แต่ชื่อตัวแปรใช้ `CamelCase`
- **Contexts:**
  - `ctx context.Context` ต้องเป็น parameter แรกเสมอ
  - ห้ามเก็บ Context ใน Struct
  - ห้ามสร้าง Custom Context type
- **Crypto/rand:** ใช้ `crypto/rand` สำหรับสร้าง key ห้ามใช้ `math/rand`

---

## การทดสอบ (Useful Test Failures)

- **Assertion Libraries:** ห้ามสร้างหรือใช้ library assertion ให้ใช้ Go ปกติ (`if got != want`) หรือไลบรารีมาตรฐานเช่น `cmp`
- **Got before Want:** พิมพ์ค่าที่ได้ก่อนค่าที่คาดหวัง (`got %v, want %v`)
- **Print Diffs:** ถ้า output ใหญ่หรือดูยาก ให้ใช้ `cmp.Diff` เพื่อแสดงความแตกต่าง
- **Table-driven tests:** แนะนำให้ใช้เมื่อมี test cases หลายกรณีที่มี logic คล้ายกัน
- **Test Helpers:** ใช้ `t.Helper()` เพื่อให้ error log ชี้ไปที่บรรทัดที่เรียกใช้ helper ไม่ใช่ในตัว helper เอง
- **Package `testing`:** ใช้แพ็กเกจ `testing` เท่านั้น ห้ามใช้ Framework อื่น

---
