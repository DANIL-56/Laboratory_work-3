# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР
Отчет по лабораторной работе №3 выполнил(а):
- Подповетный Данил Артемович
- Нмт-232203

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | ? |
| Задание 2 | * | ? |
| Задание 3 | * | ? |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


## Цель работы
Разработать оптимальный баланс изменения сложности для десяти уровней игры Dragon Picker.

## Задание 1. Начало работы с прототипом
### Ознакомьтесь с презентацией третьей лекции, оцените структуры игры Dragon Picker. Скачайте и ознакомьтесь с прототипом игры Dragon Picker на движке Unity. Запустите игровую сцену, проанализируйте движение дракона:
### - Какие переменные влияют на движение дракона на сцене? Укажите эти переменные.

Ход работы:
- Движение дракона на сцене зависит от его скорости, а также случайного фактора, определяющего момент смены траектории.

### - Какие переменные влияют на сложность игры на сцене? Укажите эти переменные.

Ход работы:
- Сложность игры определяется следующими параметрами: скорость движения дракона, частота появления яиц, скорость их падения, количество щитов, частота изменения направления дракона и размер щита.

## Задание 2. Начало работы с шаблоном google-таблицы для балансировки игры
### Ознакомьтесь с презентацией третьей лекции, оцените структуры шаблона таблицы для балансировки. Отметьте, как может быть использован шаблон таблицы для визуализации изменения уровней сложности в игре Dragon Picker?

Ход работы:
- Шаблон таблицы можно использовать для отслеживания изменений переменных "баланса" на каждом уровне. Кроме того, для удобства их можно представить в виде различных графиков.

## Задание 3

### Задание 3.1 Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице. 

Ход работы:
- Для работы были выбраны следующие переменные: скорость дракона, частота спавна яиц и количество слоёв щита.
- Автоматическое заполнение таблицы:

```rd
            import gspread
      
      gc = gspread.service_account(filename='dragonpickeranalis-75d63a0aadb4.json')
      sh = gc.open("Unity Лаба 3")
      
      def update_indexs():
          for i in range(1, 11):
              sh.sheet1.update_acell(f"A{i+1}", str(i))
      
      def update_dragon_speed(percent: str):
          current_speed = float(sh.sheet1.acell("B2").value)
          multiplier = 1 + float(percent) / 100  # Преобразуем процент в множитель
          for i in range(3, 12):
              current_speed *= multiplier
              sh.sheet1.update_acell(f"B{i}", f"{current_speed:.2f}")  # Форматируем число
      
      def update_time_egg_drop(percent: str):
          tail = (100 - int(percent)) / 100  # Преобразуем процент в множитель
          current_time = float(sh.sheet1.acell("C2").value)
          for i in range(3, 12):
              current_time *= tail
              sh.sheet1.update_acell(f"C{i}", f"{current_time:.2f}")  # Форматируем число
      
      def update_shields_count():
          current_count = int(sh.sheet1.acell("E2").value)
          repeat = 1
          for i in range(3, 12):
              sh.sheet1.update_acell(f"E{i}", str(current_count))
              repeat += 1
              if repeat == 3:
                  current_count -= 1
                  repeat = 0
      
      def update_egg_speed(percent: str):
          multiplier = 1 + float(percent) / 100
          current_speed = float(sh.sheet1.acell("D2").value)
          for i in range(3, 12):
              current_speed *= multiplier
              sh.sheet1.update_acell(f"D{i}", f"{current_speed:.2f}")
      
      if __name__ == "__main__":
          update_indexs()
          update_dragon_speed("20")
          update_time_egg_drop("10")
          update_egg_speed("5")
          update_shields_count()
```



