# state_management

## Pembahasan:
  - ### Ephemeral State
    Ephemeral State adalah state management yang hanya dapat digunakan spesifik dalam 1 widget atau bersifat lokal.

    ```dart
    class CounterWidget extends StatefulWidget {
      const CounterWidget({super.key});

      @override
      _CounterWidgetState createState() => _CounterWidgetState();
    }

    class _CounterWidgetState extends State<CounterWidget> {
      int _counter = 0;

      @override
      Widget build(BuildContext context) {
        return Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text('Counter Value: $_counter'),
              const SizedBox(height: 10),
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    _counter++;
                  });
                },
                child: const Text('Increment'),
              ),
            ],
          ),
        );
      }
    }
    ```

    Seperti contoh diatas, management statenya cuma dapat dilakukan pada widget Increment Button saja.

  - ### App State
    App State merupakan state menagement global yang artinya dapat digunakan dalam 1 aplikasi. Berikut adalah contoh codenya:

    ```dart
    class CounterModel extends Model {
      int _counter = 0;

      int get counter => _counter;

      void increment() {
        _counter++;
        notifyListeners();
      }

      void decrement() {
        if (_counter >= 1) {
          _counter--;
        }
        notifyListeners();
      }
    }
    ```
    Jadi disini class CounterModel berisi variabel counter, method increment() dan decrement() yang berfungsi untuk melakukan state management.

    ```dart
    class CounterWidget extends StatelessWidget {
      const CounterWidget({super.key});

      @override
      Widget build(BuildContext context) {
        return ScopedModelDescendant<CounterModel>(
          builder: (context, child, model) => Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                Text('Counter Value: ${model.counter}'),
                const SizedBox(height: 10),
                ElevatedButton(
                  onPressed: () {
                    model.increment();
                  },
                  child: const Text('Increment'),
                ),
                ElevatedButton(
                  onPressed: () {
                    model.decrement();
                  },
                  child: const Text('Decrement'),
                ),
              ],
            ),
          ),
        );
      }
    }
    ```

    Lalu disini terdapat class CounterWidget yang menampilkan value Counter dan juga tombol untuk merubah value Counter. value counter tersebut diambil dari class CounterModel yang sudah disebutkan sebelumnya.

    <br>

    Nah lalu jika saya punya widget lain yang ingin menampilkan atau mengubah nilai Counter, saya bisa memanggilnya dari class CounterModel.

## Kesimpulan:
Dalam case User Authentication, biasanya ketika kita selesai melakukan authentication di aplikasi maka kita akan mendapatkan response dari server yang mungkin bisa berupa data diri atau data lain. Nah jadi kita bisa menggunakan App State Management untuk mengontrol data response dari server yang akan kita tampilkan pada widget lain.

<br>

Sedangkan untuk case Shopping Cart, pada saat kita ingin memasukkan barang yang ingin ke beli ke dalam Cart, kita harus menekan Button masukkan ke Cart terlebih dahulu, yang artinya kita harus passing datanya ke widget lain yaitu dari Button ke Shopping Cart. Disinilah App State Management bekerja untuk mengontrol data antar Widget bahkan dalam satu aplikasi.