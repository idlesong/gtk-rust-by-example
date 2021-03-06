# Поля, панели, прокручиваемые окна и просмотр текста

## GtkPaned

Это контейнеры, которые могут быть ориентированы вертикально или горизонтально,
представляют собой два элемента, размер которых может изменяться.
Размер этих двух элементов может быть изменен простым нажатием и перемещением
разделяющей полосы между ними.

```rust
let container = Paned::new(Orientation::Horizontal);
let left_widget = ...;
let right_widget = ...;
container.pack1(&left_widget, true, true);
container.pack2(&right_widget, true, true);
```

## GtkEntry
Элементы позволяют пользовательскому интерфейсу принимать строку текста как
входное значение, что может быть использовано другими виджетами для выполнения
некоторых действий, используя данный текст как входные данные.
```rust
let entry = Entry::new();
entry.set_text("Some Text");
if let Some(text) = entry.get_text() {
    println!("{}", text);
}
```
## GtkTextView

Текстовые панели нужны для двух вещей:
- способность показывать многострочный текст
- возможность пользователю вводить многострочный текст
Текстовая панель может быть настроена так, что ее содержимое нельзя
редактировать, если есть такая необходимость. Также есть возможность
настраивать работу с переносами текста. Текстовые панели не умеют работать
с форматированным текстом, однако вполне могут быть использованы как
редактор кода. Если вы хотите, чтобы текст был показан в виде HTML, смотрите
**GtkWebView**, если же вы хотите получить редактор кода, смотрите
**GtkSourceView**.

> Заметьте, что часто бывает лучше создать и привязать **GtkTextBuffer**
  к вашему текстовому полю вручную, чтобы получить указатель на буфер,
  который вы можете хранить, и избежать непрямого обращения, когда вы
  программируете ваш пользовательский интерфейс (UI). Имея указатель на буфер,
  можно легко получить доступ к тексту, который содержится в текстовой панели.

```rust
// Буфер для текстовой панели с None в качестве параметра, потому что мы не
// собираемся определять никаких текстовых тэгов для этого буфера.
let text_buffer = TextBuffer::new(None);
// После этого мы должны присвоить буфер новой текстовой панели, которая будет
// самостоятельно обновлять себя при добавлении или удалении текста из буфера.
let text_view = TextView::new_with_buffer(&text_buffer);
```

Извлечение текста из **GtkTextBuffer** требует некоторой сноровки, так что
мы привели пример функции, которую вы можете использовать для того, чтобы
получить содержимое буфера в виде строки (String). Вы можете указать
определенный участок текста, который будет извлечен из буфера.
```rust
/// Получить все содержимое буфера в строковом представлении.
fn get_buffer(buffer: &TextBuffer) -> Option<String> {
    let start = buffer.get_start_iter();
    let end = buffer.get_end_iter();
    buffer.get_text(&start, &end, true)
}
```

## GtkScrolledWindow
Это одноэлементные контейнеры которые предоставляют прокручиваемые окна внутри
них. Часто бывает удобным сочетать их вместе с текстовыми полями, которые
возможно прокручивать. Это как раз то, что мы хотим сделать в этой главе.
```rust
let scrolled_window = ScrolledWindow::new(None, None);
scrolled_window.add(&text_view);
```