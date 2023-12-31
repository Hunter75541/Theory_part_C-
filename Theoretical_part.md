1. **Пространство имён**
      * Что такое пространство имён в C++ и какое его назначение?
      Каждый программный объект имеет несколько атрибутов, например, такие как: область действия, блок, класс, функция, и др. Среди них имеется
      атрибут, называемый поименованной областью, который позволяет явным образом задать область определения имен (namespace) как части глобальной
      области видимости, которая позволяет избегать конфликтов имен. Это бывает удобно для ситуаций, когда программа достаточно большая, причем
      пишущаяся разными разработчиками. В таком случае целесообразно произвести отделение кода, написанного одним человеком, от кода, написанного
      другим (чтобы избежать совпадений и доступа к ненужным средствам), для ликвидации конфликта имен и при работе с раздельной компиляцией (заголовочник, файл с реализацией, мэйн)
      
      namespace [имя_области]{ /* Объявления */ }
      
      namespace a_name 
      {
         class A 
         {
            ...
         }; 
         void f();  
         ... 
      }
      
      a_name::A
      
      Все содержимое стандартной библиотеки (функции, классы, шаблоны, объекты) объявлено в рамках пространства имен std. Полное имя любого
      элемента стандартной библиотеки состоит из имени пространства имен, оператора разрешения области видимости :: и собственного имени элемента
      
      #include <cmath> 
      double x = std::sqrt(2.0);
      
      * Какие преимущества могут быть получены при использовании пространства имен STL?

2. **Функции**
   * Что такое перегрузка функций в C++? Приведите примеры.
      Учитывая, что язык программирования C++ по максимуму использует ООП, а значит, работать в нем приходится с пользовательскими типами данных (объектами классов), то для комфортной работы с ними, как ровно и со стандартными типами, используются как имеющиеся функции, так и вновь определенные. Поскольку часто возникают ситуации, в которых одна и та же функция может работать с разным количеством аргументов или с аргументами различного типа при схожем принципе/смысле работы (механизм работы схожий, а семантика различна), удобно воспользоваться переопределением функции или, так называемой, перегрузкой.
         В основном, функция считается перегруженной, если ее имя остается неизменным, а изменяемы:
         1) Число аргументов
         2) Тип аргументов
         3) Квалификаторы const или volatile
         При компиляции программы компилятор выбирает для вызова ту из перегруженных функций, которая наилучшим образом соответствует объявленной функции с учетом количества и типа передаваемых ей аргументов. 
         
         double Norma(double x, double y)
         {
            cout << "(function 1)";
            return sqrt(x*x + y*y);
         }
         
         double Norma(double x, double y, double z)
         {
         cout << "(function 2)";
         return sqrt(x*x + y*y + z*z);
         }
         
         double Norma(double x, int y)
         {
         cout << "(function 1)";
         return sqrt(x*x + y*y);
         }
         
         double Norma(int x, float y, double z)
         {
         cout << "(function 2)";
         return sqrt(x*x + y*y + z*z);
         }
         ....
         
3. **Классы и структуры**
   * В чем различие между классом и структурой в C++?
   Рассмотрим структуру Student, состоящую из трех член-данных: 
   
   struct A
   { 
         char a[128]; 
         int b; 
         double c; 
   };
   
   Например, мы хотим реализовать метод для вывода информации по данному студенту:
   
   void ShowStudent(const Student * p)
   {
         cout << p->name << “ ” << p->sem << “ ” << p->stip << “\n”;
   }
   
   В случае структур, мы используем процедурный подход, а именно, описываем типы данных и определяем функции, предназначенные для работы с ними. Однако, следует заметить, данные отделены от функций осуществляющих работу с ними. (принципиально, это обстоятельство побудило разработчиков языка С осуществить его модификацию, реализовавшуюся в виде языка С++). Таким образом, осуществлен переход от процедурного программирования к объектно-ориентированному программированию (ООП), в котором функции для работы с данными являются частью их типа - класса. Класс  представляет собой микс из членов-данных (полей) и членов-функций (методов), причем важно отметить, что все поля класса по-умолчанию являются недоступными для пользователя (private), в отличие от struct (где они public), что минимизирует возможность нежелательного/ошибочного (несанкционированного/намеренного) изменения их содержимого (эта концепция образует одну из 3-х парадигм программирования ООП как иннкапсуляцию):
      class A 
      {
         char obj[128];  // private
         int a; //private
         double c;  //private
         void show();  // private
      };
      В остальном классы и структуры эквивалентны (за исключением модификатора по умолчанию при наследовании). Тип struct в C++ может использоваться также, как и в языке С, как запись без методов, все члены-данные которых публичны.
   
   * Чем отличается объявление класса от его определения?
      Объявление класса не подразумевает передачу компилятору информации о содержании класса (его полях, методах):
      class Student;
      Однако определение уже явно говорит о том, какие и сколько полей есть в классе, какого они типа (а следовательно, сколько памяти потенциально под них нужно будет выделить), какие методы необходимы для работы с ними. 
      class Student
      {
         char name[128];
         int sem;
         double stip;
         void show();
      };
   
   * Какие уровни доступа вы знаете в C++? Расскажите о каждом из них.
   
   Как уже было отмечено в предыдущем пункте, если разрешить прямой доступ к членам-данным класса, то возможна случайная (или намеренная) «порча» объекта, при которой его данные могут потерять смысл, как следствие, нарушить логику работы программы, которую изначально задумывал разработчик и даже, вероятно, поставить под угрозу работу других программ. Иногда говорят, что класс должен гарантировать выполнение контракта для объекта класса в течение всего времени его жизни, поэтому, как правило, прямая модификация членов-данных запрещена.
      Однако, в зависимости от задач, для большей гибкости ООП, у всех членов класса имеются квалификаторы доступа: public , protected или private
      Публичные (public) члены класса доступны как методам класса, так и внешним функциям. 
      Приватные (private) члены класса доступны только методам класса и друзьям класса. 
      Cуществует также 3 квалификатор - защищенный (protected). Различие между (private) приватными и (protected) защищенными членами проявляется  при наследовании классов, а именно, доступ к защищенным полям может быть осуществлен методами и друзьями (классами или методами) родительского класса, кроме того, они могут использоваться производными классами данного класса. 
      Порядок следования блоков public, protected и private, а также их количество не имеют значения.
      
      class A 
      { 
            int i; //приватная переменная
         public: 
            void show() const; // публичный метод
         private: 
            void f();  // приватный метод 
      };
   
   
   * Что такое дружественные функции и классы?
   Дружественные функции – это функции, объявленные в теле класса с модификатором friend. Они имеют доступ к приватной части класса, причем такие функции не являются членами класса и вызываются как обычные функции (не получая указателя this, в отличие от методов класса). Дружественные функции бывает удобно использовать, когда первый аргумент функции обязан иметь встроенный тип (например, при перегрузке оператора << ) или когда функция должна иметь доступ к приватной части нескольких классов (тогда ее можно сделать дружественной для них).
   
   friend double dist(const Point& p, const Line& obj); // объявление в классе 
   
   double dist(const Point& p, const Line& obj ) // реализация вне его
   { 
      return abs(obj.a * p.x + obj.b * p.y + obj.c) / sqrt(obj.a * obj.a + obj.b * obj.b);
   }
   
   Если дружественной функцией может быть метод другого класса. Если же все методы иного класса по отношению к данному являются дружественными функциями, то такой класс - дружественный (по отношению к данному) класс. 
   
4. **Массивы**

   * Как объявить и использовать массивы в C++?
   
   Массивы могут быть статическими или динамическими (фактически, выбор идет в пользу удобного для реализации данной задачи варианта).
   В случае, если нам нужен статический массив, то его размер обязан быть строго фиксированным!!! (поскольку компилятор, на момент осуществления компиляции, должен понимать сколько ему нужно под данный тип массива выделить памяти). В целом, данный вариант подходит для решения задач, в которых нам априорно известно кол-во элементов, потенциально необходимых для заполнения массива.
   Иная же ситуация, когда нам изначально размер неизвестен (наиболее частая ситуация), тогда требуется использовать динамический массив, поскольку в этом случае его размер будет определяться в процессе компиляции программы. (задачи считывания данных из файла, с IPшника). 
   // статический массив массивов (или двумерный массив, расположенный непрерывно в памяти)
   
   int arr [3][2]; 
   for (int i = 0; i < 3; ++i)
   {
      for (int j = 0; j < 2; ++j)
      {
         arr[i][j] = i + j; 
      }
   }
   
   // динамический массив 
   
   int *arr , n =10000; // уже динамически (задаем указатель типа инт) и тут ( для примера) знаем кол-во элементов
   arr = malloc (n * sizeof(arr[0])); // выделяем под заданное кол-во элементов память динамически
   if (!arr)  // обязательная проверка на случай, если что-то пошло не так
   {
      printf (“Allocation failed n”); 
      exit(1);
   }
   // ...
   free(arr); // не забываем сами очистить память, поскольку автоматического высвобождения нет и вся ответственность ложиться на программиста
   
   
   Привожу пример работы программы (уже двумерный динамический), заполняющей двумерный массив случайными числами [0;10]
   
   #include <iostream>
   #include <stdlib.h>
   #include <ctime>
   
   using namespace std;
   
   void fill_arr (int** arr, int const rows, int const cols)
   {
      for(int i=0;i<rows;++i)
         {
            for(int j=0;j<cols;++j)
            {
               arr[i][j] = rand() % 10;
            }
         }
   }
         
   void Print_arr (int** arr, int const rows, int const cols)
   {
      for(int i=0;i<rows;++i)
      {
         for(int j=0;j<cols;++j)
         {
            cout << arr[i][j] << "\t";
         }
         cout << endl;
      }
   }
      
   int main(int argc, char *argv[]) 
   {
      srand(time(NULL)); 
      int rows = 5 , cols = 6;
      int **arr = new int *[rows]; // выделение памяти на указатели строк, каждый из которых - указатель на столбец
      for(int i=0; i< rows; ++i)   // выделение памяти под каждый указатель, на который ссылается указатель строки ( выделение под столбцы по каждой строке)
      {
         arr[i] = new int[cols];
      }
      if(!arr)
         cout << "Allocation is failed!";
      fill_arr(arr, rows, cols);
      Print_arr(arr, rows, cols);
      // высвобождение памяти 
      for(int i=0; i< rows; ++i)
      {
         delete [] arr[i];
      }
      delete [] arr;
      

5. **Ссылки и указатели**
   * В чем разница между ссылками и указателями в C++?
   Указатель - это семейство типов, предназначенных для хранения адреса ячейки оперативной памяти (байта). Размер (в байтах) указателей одинаков и зависит от среды. С указателями определены две основные операции: унарная операция взятия адреса & (применяется к переменным и служит для инициализации) и унарная операция взятия значения по адресу (т.н. разыменование) * (служит для получения переменной, на которую ссылается указатель). 
   Ссылка же, по сути, во многом аналогична указателю. Для того, чтобы ее использовать, ровно как и указатель, ее нужно инициализировать, после чего ссылка становится синонимом переменной (фактически, все потенциально совершаемые действия с ссылкой реально происходит с переменной, на которую она ссылается).
   Разница между ними состоит лишь в том, что ссылки не требуют оператора * (разыменования) для доступа к значению переменной, что значительно упрощает работу с функциями (при передаче аргумента в функцию и последующей работой с ним), а также они не могут не ссылаться на объект (указатель на указатель на объект).
   
   * Как работают указатели на указатели? Приведите примеры.
     Как и любой тип, указатель может быть «целью» другого указателя. Так, возможны типы «указатель на int», «указатель на указатель на int » и т.д.. Поэтому, int **, как и любой другой указатель, хранит адрес байта, содержимое которого (вместе с несколькими последующими байтами типа на который осуществляется указание) трактуется как адрес целого числа.
      Грубо говоря:
      
      int i = 2, *pi = i , **ppi = &pi
      **ppi = 4; // *pi = 4;  i = 4;  ( тоже самое)
      
      В большинстве случаев удобно работать с указателями на указатель, когда речь идет о многомерных динамических массивах.
      
      int arr[2][3] = {{1, 2, 3}, {4, 5, 6}}; 
      // arr[0] == *arr , arr[1] == *(arr + 1)
      **arr = -1;  // arr[0][0] = -1;
      *(*arr + 1) = 2; // arr [0 ][1] = 2; 
      
      и далее, обобщая, можно получать доступ к элементам массива без оператора [], а непосредственным обращением по адресу в памяти
      
      // *(*(arr + i) + j ) = arr[i][j]
      
      В частности, пример в котором демонстрируется работа указателя на указатель приведен в п. 4 ("Массивы")

6. 6. **Обработка исключений**
      * Что такое обработка исключений? Какие ключевые слова используются для обработки исключений в C++?
      
      Рассмотрим для выявления проблемы и необходимости использования обработки исключений следующий пример. Пусть имеется класс, осуществляющий работу с трехмерными векторами:
      
      class Vec3d 
      { 
         double x, y, z; 
         public:
            double& operator[](int idx); 
      };
      
      Рассмотрим перегрузку оператора []
      
      double& Vec3D::operator[](int idx)
      { 
         if (idx == 0) 
            return x; 
         else if (idx == 1) 
            return y;
         else if (idx == 2) 
            return z;
         exit(42);
         
      }
      
      В случае, когда аргумент больше 2 или меньше 0, результат выполнения программы неоднозначен, а именно, по-факту, обращение по такому индексу будет являться ошибкой, но несмотря на это, пусть все-таки функция что-то сделает. Пусть, например, завершится аварийно, возвращая код ошибки 42. Однако, в этом случае придется передавать код ошибки от функции, в которой она произошла, к функции, в которой она обрабатывается, по всей цепочке вызовов, + возникает ряд вопросов: насколько код уникален и как его интерпретировать? Сколько функций в библиотеках захотят вернуть такой код? всегда ли можно вернуть код ошибки?. Итого, суммируя, проблема состоит в том, что ошибка совершается в пользовательском коде (пользователь запросил доступ к несуществующему элементу массива), становится явной в библиотечном (внутри метода operator[] ), а обрабатываться должна снова в пользовательском (чтобы пользователь мог решить, какое сообщение выводить, завершать ли программу и прочее). Для решения упомянутых проблем как раз и выступает универсальный механизм исключений (exception). 
         Синтаксически исключение является объектом произвольного типа (встроенного или определенного создателем библиотеки). Библиотечный код генерирует (throw) исключение, а пользовательский перехватывает его (catch) и обрабатывает. Если было сгенерировано исключение (любого типа), функция завершается, как если бы был выполнен return; , без возврата значения. Затем выполняется выход из функции, вызвавшей проблемную, и так далее до тех пор, пока исключение не будет перехвачено или не будет выполнен выход из main.
         
         void print(const Vec3D& v) 
         { 
            try 
            { 
               cout << v[3]; 
            } 
            catch (const EVec3DBadIndex& e)
            { 
               cerr << “Oops! Bad index ” << e.getIdx(); 
            } 
         }
         // Также вместо одного catch-блока может следовать их последовательность
         try 
         { 
            /* block 0 */ 
         } 
         catch (const E1& e) 
         { 
            /* block 1 */ 
         } 
         catch (const E2& e) 
         { 
            /* block 2 */ 
         } 
         catch (...) 
         { 
            /* block final */ 
         }
         Иногда требуется, чтобы исключение было обработано сразу в нескольких местах. Для этого используется повторная генерация того же исключения в catch-блоке с помощью выражения throw, но уже без объекта-исключения
         
         void f() 
         {
            throw E();
         }
         
         void g() 
         {
            try 
            { 
               f(); 
            }
            catch(const E& e)
            {
               cerr << 1;
               throw;
            }
         }
         
         void h() 
         {
            try 
            { 
               g(); 
            }
            catch (const E& e)
            {
               cerr << 2;
            }
         }

      
