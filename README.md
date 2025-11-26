# คู่มือสไตล์การเขียนโค้ดของ Google (Google Style Guides)

[Original version(EN)](README_ORIGINAL)

โปรเจกต์โอเพนซอร์สขนาดใหญ่ทุกแห่งล้วนมีคู่มือสไตล์ (Style Guide) เป็นของตัวเอง ซึ่งก็คือชุดข้อตกลง (ที่บางครั้งอาจดูเหมือนกำหนดขึ้นมาลอยๆ) เกี่ยวกับวิธีการเขียนโค้ดในโปรเจกต์นั้นๆ การที่โค้ดทั้งหมดเป็นไปในทิศทางเดียวกันจะช่วยให้ทำความเข้าใจโค้ดเบสขนาดใหญ่ได้ง่ายขึ้นมาก

คำว่า "สไตล์" ในที่นี้ครอบคลุมเนื้อหาหลายด้าน ตั้งแต่เรื่องเล็กน้อยอย่าง "จงใช้ camelCase สำหรับชื่อตัวแปร" ไปจนถึงกฎเหล็กอย่าง "ห้ามใช้ตัวแปร Global" หรือ "ห้ามใช้ Exceptions" โปรเจกต์นี้ ([google/styleguide](https://github.com/google/styleguide)) ได้รวบรวมลิงก์ไปยังแนวทางปฏิบัติที่เราใช้จริงในการเขียนโค้ดที่ Google หากคุณกำลังแก้ไขโปรเจกต์ที่มีต้นกำเนิดมาจาก Google คุณอาจถูกแนะนำมาที่หน้านี้เพื่อดูสไตล์ที่เหมาะสมกับโปรเจกต์นั้น

- [AngularJS Style Guide][angular]
- [Common Lisp Style Guide][cl]
- [C++ Style Guide][cpp]
- [C# Style Guide][csharp]
- [Go Style Guide][go]
- [HTML/CSS Style Guide][htmlcss]
- [JavaScript Style Guide][js]
- [Java Style Guide][java]
- [JSON Style Guide][json]
- [Markdown Style Guide][markdown]
- [Objective-C Style Guide][objc]
- [Python Style Guide][py]
- [R Style Guide][r]
- [Shell Style Guide][sh]
- [Swift Style Guide][swift]
- [TypeScript Style Guide][ts]
- [Vim script Style Guide][vim]

นอกจากนี้ ในโปรเจกต์ยังมีไฟล์ [google-c-style.el][emacs] ซึ่งเป็นไฟล์ตั้งค่าสำหรับ Emacs เพื่อให้สอดคล้องกับสไตล์ของ Google

ในอดีตเราเคยโฮสต์เครื่องมือ `cpplint` ไว้ที่นี่ แต่เราได้หยุดการอัปเดตสู่สาธารณะแล้ว ปัจจุบันชุมชนโอเพนซอร์สได้นำโปรเจกต์ไปสานต่อ (Fork) ดังนั้นเราจึงแนะนำให้ผู้ใช้เปลี่ยนไปใช้ที่ https://github.com/cpplint/cpplint แทน

หากโปรเจกต์ของคุณจำเป็นต้องสร้างรูปแบบเอกสาร XML ใหม่ [XML Document Format Style Guide][xml] อาจช่วยคุณได้ นอกจากกฎเรื่องสไตล์แล้ว ในนั้นยังมีคำแนะนำเกี่ยวกับการออกแบบรูปแบบใหม่เทียบกับการปรับใช้รูปแบบเดิมที่มีอยู่ รวมถึงการจัดรูปแบบเอกสาร XML instance และการเลือกระหว่าง elements กับ attributes

คู่มือสไตล์ในโปรเจกต์นี้อยู่ภายใต้สัญญาอนุญาต **CC-By 3.0 License** ซึ่งสนับสนุนให้คุณนำเอกสารเหล่านี้ไปเผยแพร่ต่อได้ ดูรายละเอียดเพิ่มเติมที่ [https://creativecommons.org/licenses/by/3.0/][ccl]

คู่มือสไตล์ของ Google ต่อไปนี้ อยู่ภายนอกโปรเจกต์นี้:

- [Effective Dart][dart]
- [Kotlin Style Guide][kotlin]

เนื่องจากการดูแลโปรเจกต์ส่วนใหญ่ต้องทำผ่านระบบควบคุมเวอร์ชัน ([VCS]) การเขียนข้อความ Commit (Commit messages) ที่ดีจึงสำคัญต่อสุขภาพของโปรเจกต์ในระยะยาว โปรดดูที่ [How to Write a Git Commit Message](https://cbea.ms/git-commit/) ซึ่งเป็นแหล่งข้อมูลที่ยอดเยี่ยม แม้บทความจะพูดถึง Git [SCM] โดยตรง แต่หลักการสามารถนำไปใช้ได้กับทุกระบบ และธรรมเนียมปฏิบัติของ Git หลายอย่างก็สามารถนำไปปรับใช้กับระบบอื่นได้ง่าย

## การร่วมแก้ไข (Contributing)

โปรดทราบว่า คู่มือสไตล์เหล่านี้เกือบทั้งหมดเป็นการคัดลอกมาจากคู่มือภายในของ Google เพื่อช่วยให้นักพัฒนาที่ทำงานกับโปรเจกต์โอเพนซอร์สของ Google ทำงานได้สะดวกขึ้น การเปลี่ยนแปลงคู่มือสไตล์จะทำที่เอกสารภายในของ Google ก่อน แล้วจึงค่อยคัดลอกมายังเวอร์ชันที่ปรากฏที่นี่

**ดังนั้น เราจึงไม่เปิดรับการแก้ไขจากภายนอก (External contributions are not accepted)** Pull requests ที่ส่งเข้ามามักจะถูกปิดโดยไม่มีการคอมเมนต์ตอบกลับ

อย่างไรก็ตาม คุณสามารถแจ้งปัญหาผ่าน [GitHub tracker][gh-tracker] ได้ หากเป็นประเด็นที่ตั้งคำถาม, มีเหตุผลทางเทคนิคที่สมควรแก่การเปลี่ยนแปลง หรือชี้ให้เห็นข้อผิดพลาดที่ชัดเจน อาจมีการพูดคุยตอบกลับและนำไปสู่การเปลี่ยนแปลงได้ในทางทฤษฎี แต่ขอให้เข้าใจว่าเราปรับปรุงโดยยึดความต้องการภายในของ Google เป็นหลัก

<a rel="license" href="https://creativecommons.org/licenses/by/3.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/3.0/88x31.png" /></a>

TH

[go]: go/index_th

EN

[cpp]: https://google.github.io/styleguide/cppguide.html
[csharp]: https://google.github.io/styleguide/csharp-style.html
[swift]: https://google.github.io/swift/
[objc]: objcguide.md
[gh-tracker]: https://github.com/google/styleguide/issues
[java]: https://google.github.io/styleguide/javaguide.html
[json]: https://google.github.io/styleguide/jsoncstyleguide.xml
[kotlin]: https://developer.android.com/kotlin/style-guide
[py]: https://google.github.io/styleguide/pyguide.html
[r]: https://google.github.io/styleguide/Rguide.html
[sh]: https://google.github.io/styleguide/shellguide.html
[htmlcss]: https://google.github.io/styleguide/htmlcssguide.html
[js]: https://google.github.io/styleguide/jsguide.html
[markdown]: https://google.github.io/styleguide/docguide/style.html
[ts]: https://google.github.io/styleguide/tsguide.html
[angular]: https://google.github.io/styleguide/angularjs-google-style.html
[cl]: https://google.github.io/styleguide/lispguide.xml
[vim]: https://google.github.io/styleguide/vimscriptguide.xml
[emacs]: https://raw.githubusercontent.com/google/styleguide/gh-pages/google-c-style.el
[xml]: https://google.github.io/styleguide/xmlstyle.html
[dart]: https://www.dartlang.org/guides/language/effective-dart
[ccl]: https://creativecommons.org/licenses/by/3.0/
[SCM]: https://en.wikipedia.org/wiki/Source_control_management
[VCS]: https://en.wikipedia.org/wiki/Version_control_system
