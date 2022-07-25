public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

#1 Первым очередем в MetaSpace загружаются классы JvmComprehension,System.classes
    сначала запрос идет Apllication ClassLoader тот делегирует загрузку в Platform ClassLoader 
    тот в свою очередь делегирует загрузку в Bootstrap ClassLoader. тот если нашел классы, загружает
    если нет то передает всё в Platform ClassLoader . если он не нашел то обязанность идет в Application ClassLoader
    и в случае если он тоже не нашел то выкидывается ошибка ClassNotFoundException
    далее код идет на проверку, подготовку и на разрешение символьных знаков и далее идет 
    Инициализацияю и загрузка в Metaspace

#2 в Stack создается фрейм **"main"** для хранения значимых типов. 
   1. записывается туда int i= 1; 
   2. в куче создается ссылочный объект Object и в Stack создается объект "o" с ссылкой Object
   3. в куче создается ссылочный объект Integer со значенем = 2 и в Stack создается объект ii с ссылкой на Integer из Heap
   4. в Stack создается новый Frame **printAll**
      1. в нем создаются новый int i = 1, объект 'o' с дубликатом ссылки на Object из main, объект ii с дубликатом ссылки на Integer = 2 из кучи 
   5. где в кучу записывается ссылочный объект Integer и в Stack сохраняется объект uselessVar с ссылкой на Integer =700.

   6. в Stack создастся новый Frame **println**
      1. где создастся объект 'o' с дубликатом ссылки на Object из кучи
      2. в Stack создается новый frame **toString** где создаются все данные параметров и ссылки на данные из кучи 
      3. создается int i = 1, объект ii с дубликатом ссылки на Integer из 'main'.
   
   7. в Stack создается новый Frame **println**
      1. где в куче создается ссылочный тип String "finished"
      2. в Стак создается объект со сслыкой на String из кучи

после выполнения программы из стека начиная с конца будут удалятся фреймы. 
        1. println c данными из кучи
        2. println с 'printall' c данными из кучи
        3. printAllc данными из кучи

есть 2 метода Сборки мусора
        1. подсчет ссылок
        2. обход графа достижимых объектов