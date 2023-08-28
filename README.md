# Özel Dropdown Menü Widgeti

Bu özel Dropdown Menü Widget'i, Flutter uygulamanızda kullanabileceğiniz özelleştirilebilir bir dropdown menüdür. Bu README, widget'ın kullanımı ve özellikleri hakkında bilgi sağlayacaktır.

#### DropdownMenuWidget kodları
```dart
class DropdownMenuWidget extends StatelessWidget {
  final List items;
  final double positiony;
  final VoidCallback onClick;
  final CustomDropdownController controller;
  final BoxShadow? shadow;
  final BorderRadius? radius;
  final Color? backgroundColor;
  final Divider? divider;
  final EdgeInsets? padding;
  final Widget? child;

  const DropdownMenuWidget({
    super.key,
    required this.items,
    required this.positiony,
    required this.onClick,
    required this.controller,
    this.shadow,
    this.radius,
    this.backgroundColor,
    this.divider,
    this.padding,
    this.child,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        boxShadow: [
          shadow ??
              BoxShadow(
                color: Colors.grey.withOpacity(0.5),
                spreadRadius: 0,
                blurRadius: 30,
                offset: const Offset(0, 0),
              ),
        ],
        borderRadius: radius ?? BorderRadius.circular(15),
        color: backgroundColor ?? Colors.amber,
      ),
      child: SingleChildScrollView(
        child: Column(
          children: items
              .map((item) => Column(
                    children: [
                      items.first == item
                          ? const SizedBox.shrink()
                          : divider ??
                              const Divider(
                                height: 0,
                                color: Colors.black26,
                              ),
                      GestureDetector(
                        onTap: () {
                          controller.setSelectedValue(item);
                          onClick();
                        },
                        child: Container(
                          alignment: Alignment.centerLeft,
                          padding: padding ??
                              const EdgeInsets.symmetric(
                                  horizontal: 16, vertical: 12),
                          child: child ?? Text(item),
                        ),
                      ),
                    ],
                  ))
              .toList(),
        ),
      ),
    );
  }
}
```

## Kullanım

Widget'ı kullanmak için aşağıdaki adımları izleyebilirsiniz:

Widget'ı açacağınız konumu tıklanabilen widget'ın konumuna göre yapmak istiyorsanız tıklanabilir widget için bir [GlobalKey] tanımlayın:

```dart
GlobalKey globalKey = GlobalKey();
```
#### İlgili sayfanın başlangıcında `CustomDropdownController` oluşturun:

```dart
CustomDropdownController genderDropdownController = CustomDropdownController();
```
#### Özel Dropdown Menüyü bir TextField veya başka bir tıklanabilir widget ile birlikte kullanabilirsiniz. Örnek olarak:

```dart
TextField(
  key: globalKey,
  controller: genderController,
  onTap: () {
    toggleDropdownGender();
    findDropdownData(globalKey);
  },
  readOnly: true,
),
```
#### Dropdown Menüyü yerleştirin ve CustomDropdownController'ı iletişim kurması için verin:

```dart
Positioned(
  right: 10,
  left: 10,
  top: positiony + 30,
  child: DropdownMenuWidget(
    controller: genderDropdownController,
    items: genders,
    positiony: positiony,
    onSelect: () {
      read.selectGender();
    },
  ),
),
```
#### Dropdown Menü Widget'ınızı özelleştirebilirsiniz. Aşağıdaki özellikleri kullanabilirsiniz:

* shadow: Kutunun gölgesini belirler.
* radius: Kutunun köşe yuvarlama değerini belirler.
* backgroundColor: Menü arka plan rengini belirler.
* divider: Öğeler arasına eklenen ayırıcıyı belirler.
* padding: Öğelerin iç içe yerleştirilme şeklini belirler.
* child: Özel bir içerik eklemek için kullanılır.

## Özel Dropdown Menü Controller
CustomDropdownController, dropdown menü ile tıklanabilir widget arasındaki iletişimi sağlar. Seçilen değeri saklayabilir, temizleyebilir ve seçilen değeri kontrol edebilirsiniz.

* setSelectedValue: Seçilen değeri ayarlar.
* selectedValue: Seçilen değeri alır.
* clearSelectedValue: Seçilen değeri temizler.
* isSelectedValue: Belirli bir değerin seçilip seçilmediğini kontrol eder.


#### Widget konumunu almak için örnek fonksiyon:
```dart
  void findDropdownData(GlobalKey globalKey) {
    if (globalKey.currentContext != null) {
      final RenderBox box =
          globalKey.currentContext!.findRenderObject() as RenderBox;
      height = box.size.height;
      width = box.size.width;
      final Offset localOffset = box.globalToLocal(Offset.zero);
      positionx = localOffset.dx;
      positiony = -localOffset.dy;
    }
  }
```


