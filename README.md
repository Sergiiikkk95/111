#include<iostream>
#include<windows.h>
#include<stdlib.h>
#include<time.h>
#include<string>
using namespace std;

struct List 
{
	double usefull;
	List *next=nullptr;
};

List* create_elem(double val)//создать элемент
{
    List* result = new List();//выделяется память под обьект структуры для элемента списка
    result->usefull = val;//в поле для полезного значения сохр переданное число
    return result;//возвращаем указатель на обьект для дальнейшего использования
}

void insert(List*& head, List* val)//вставка элемента
{
    val->next = head;
    head = val;
}

void print_list(List* head)// печать списка
{
    cout << "\x1b[35m Head ";
    List* begunok = head;
    while (begunok != nullptr)
    {
        cout << " \x1b[33m->\x1b[35m [" << begunok->usefull << "]";
        begunok = begunok->next;
    }
    cout << " \x1b[33m->\x1b[35m end;\x1b[36m\n\n\n";
}

//Поиск элемента по индексу

List* find_by_index(List* head, int index)
{
    List* curr = head;
    int count = 0;
    while (curr != nullptr && count != index)
    {
        curr = curr->next;
        count++;
    }
    if (count == index)
    {
        return curr;
    }
    else
    {
        return nullptr;
    }
}

//Сравнение двух списков на равенство
bool compare_lists(List* head1, List* head2)
{
    List* curr1 = head1;
    List* curr2 = head2;
    while (curr1 != nullptr && curr2 != nullptr)
    {
        if (curr1->usefull != curr2->usefull)
        {
            return false;
        }
        curr1 = curr1->next;
        curr2 = curr2->next;
    }
    if (curr1 == nullptr && curr2 == nullptr)
    {
        return true;
    }
    else
    {
        return false;
    }
}

//Копирование списка
List* copy_list(List* head)
{
    List* new_head = nullptr;
    List* curr = head;
    List* prev = nullptr;
    while (curr != nullptr)
    {
        List* new_elem = create_elem(curr->usefull);
        if (prev == nullptr)
        {
            new_head = new_elem;
        }
        else
        {
            prev->next = new_elem;
        }
        prev = new_elem;
        curr = curr->next;
    }
    return new_head;
}

//Подсчет количества элементов в списке
int count_elems(List* head)
{
    int count = 0;
    List* curr = head;
    while (curr != nullptr)
    {
        count++;
        curr = curr->next;
    }
    return count;
}



//удаление всего списка
void delete_list(List*& head)    
{
    List* curr = head; //Создание указателя curr и инициализация его значением head - началом списка
    while (curr != nullptr)//Итеративный проход по списку с помощью цикла while, который продолжается, пока curr не равен nullptr.
    {
        List* temp = curr;//Внутри цикла создается указатель temp для сохранения адреса текущего элемента, который нужно удалить, 
        curr = curr->next;//после чего curr перемещается на следующий элемент списка.
        delete temp;//Удаляется элемент списка, на который указывает temp, с помощью оператора delete.
    }
    //Повторяем шаги 3-4 до тех пор, пока не дойдем до конца списка.
    head = nullptr;//становка указателя head в nullptr, чтобы можно было использовать его для создания нового списка или обозначения пустого списка.
}

//удаление элемента по значению
void delete_elem(List*& head, double val)  
{
    List* curr = head;//Создание указателей curr и prev и инициализация их значением head и nullptr соответственно.
    List* prev = nullptr;
    while (curr != nullptr)//Итеративный проход по списку с помощью цикла while, который продолжается, пока curr не равен nullptr.
    {
        if (curr->usefull == val)//Проверка, равно ли поле usefull у текущего элемента значению, переданному в качестве входного параметра. 
            //Если да, переходим на шаг 4.
        {
            if (prev == nullptr)//Если prev равно nullptr (т.е. мы удаляем первый элемент списка), то голова списка переопределяется на next текущего элемента.
            {
                head = curr->next;
            }
            else//В противном случае, next предыдущего элемента устанавливается равным next текущего элемента, тем самым пропуская текущий элемент.
            {
                prev->next = curr->next;
            }
            delete curr;//Элемент, на который указывает curr, удаляется с помощью оператора delete.
            return;//Возвращаем управление программе. Если элемент был найден и удален, то return; прерывает работу функции. 
        }
        prev = curr;//Если же такой элемент в списке не найдено, то функция завершает работу без каких-либо изменений списка.
        curr = curr->next;
    }
}

//Очистка списка

void clear_list(List*& head)
{
    while (head != nullptr)
    {
        List* temp = head;
        head = head->next;
        delete temp;
    }
}

struct Stack
{
    List* head = nullptr;
    void push(double val)
    {
        insert(head, create_elem(val));
    }
    double pop()
    {
        double result = head->usefull;
        delete_elem(head, result);
        return result;
    }
    bool is_empty()
    {
        return (head == nullptr);
    }
    void clear()
    {
        clear_list(head);
    }
};

//Реализация очереди на основе списка

struct Queue
{
    List* head = nullptr;
    List* tail = nullptr;
    void enqueue(double val)
    {
        List* new_elem = create_elem(val);
        if (tail == nullptr)
        {
            head = new_elem;
            tail = new_elem;
        }
        else
        {
            tail->next = new_elem;
            tail = new_elem;
        }
    }
    double dequeue()
    {
        double result = head->usefull;
        List* temp = head;
        head = head->next;
        if (head == nullptr)
        {
            tail = nullptr;
        }
        delete temp;
        return result;
    }
    bool is_empty()
    {
        return (head == nullptr);
    }
    void clear()
    {
        clear_list(head);
        tail = nullptr;
    }
};

int main()
{
	setlocale(LC_ALL, "ru");
	HANDLE hConsoleHandle = GetStdHandle(STD_OUTPUT_HANDLE);   
	SetConsoleTextAttribute(hConsoleHandle, 11);
    
    List* my_present = nullptr; // Пустой список
    
    insert(my_present, create_elem(6.67e-11));
    print_list(my_present);
    
    insert(my_present, create_elem(9.8));
    print_list(my_present);
   
    insert(my_present, create_elem(-1.35));
    print_list(my_present);
    
    List* result = find_by_index(my_present, 2);
    if (result != nullptr)
    {
        cout << "\x1b[37mЗначение элемента с индексом 2 = \x1b[36m" << result->usefull << endl<<endl;
    }
    else
    {
        cout << "\x1b[31mЭлемент с индексом 2 не найден!\x1b[36m" << endl<<endl;
    }

    List* my_list1 = nullptr;
    List* my_list2 = nullptr;

    //Заполнение первого списка
    insert(my_list1, create_elem(10));
    insert(my_list1, create_elem(20));
    insert(my_list1, create_elem(30));

    //Заполнение второго списка
    insert(my_list2, create_elem(10));
    insert(my_list2, create_elem(20));
    insert(my_list2, create_elem(30));

    //Сравнение списков на равенство
    if (compare_lists(my_list1, my_list2))
    {
        cout << "\x1b[33mСписки равны\x1b[36m" << endl << endl;
    }
    else
    {
        cout << "\x1b[31mСписки не равны\x1b[36m" << endl << endl;
    }

    //Копируем список
    List* my_list1_copy = copy_list(my_list1);

    //Печатаем оригинальный список
    cout << "\x1b[34mОригинальный список:\x1b[36m" << endl;
    print_list(my_list1);

    //Печатаем копию списка
    cout << "\x1b[34mКопия списка:\x1b[36m" << endl;
    print_list(my_list1_copy);
    
    
    List* my_list = nullptr;
    //Добавляем элементы в список
    insert(my_list, create_elem(10));
    insert(my_list, create_elem(20));
    insert(my_list, create_elem(30));
    //Подсчитываем количество элементов в списке
    int count = count_elems(my_list);
    cout << "Количество элементов в списке: " << count << endl << endl;
    //Очищаем список
    clear_list(my_list);
   
    //Очистка списков
    delete_list(my_list1);
    delete_list(my_list2);

    clear_list(my_list1);
    clear_list(my_list2);

    delete_elem(my_present, -1.35);
    print_list(my_present);
    
    delete_list(my_present);
    print_list(my_present);

    Stack my_stack;
    my_stack.push(10);
    my_stack.push(20);
    my_stack.push(30);

    cout << "\x1b[31mВыталкиваем элементы из стека:\x1b[36m ";
    while (!my_stack.is_empty())
    {
        cout << my_stack.pop() << " ";
    }

    
    Queue my_queue;
    my_queue.enqueue(10);
    my_queue.enqueue(20);

    cout << endl << endl;
	return 999;
}
