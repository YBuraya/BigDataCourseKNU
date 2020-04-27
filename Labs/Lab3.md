#Лабораторна робота № 3. Зчитування даних з WEB. 
В цій лабораторній роботі необхідно зчитати WEB сторінку з сайту IMDB.com зі 100 фільмами 2017 року виходу за посиланням «http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature». 
Необхідно створити data.frame «movies» з наступними даними: номер фільму (rank_data), назва фільму (title_data), тривалість (runtime_data). Для виконання лабораторної рекомендується використати бібліотеку «rvest». CSS селектори для зчитування необхідних даних: rank_data: «.text-primary», title_data: «.lister-item-header a», runtime_data: «.text-muted .runtime». Для зчитування url використовується функція read_html, для зчитування даних по CSS селектору – html_nodes і для перетворення зчитаних html даних в текст - html_text. Рекомендується перетворити rank_data та runtime_data з тексту в числові значення. При формуванні дата фрейму функцією data.frame рекомендується використати параметр «stringsAsFactors = FALSE».
```r
library(xml2)
library(rvest)
html_page <- read_html("https://www.imdb.com/search/title/?count=100&release_date=2017,2017&title_type=feature")
rank_data <- html_text(html_nodes(html,'.text-primary'))
rank_data <- as.numeric(rank_data)
title_data <- html_text(html_nodes(html,'.lister-item-header a'))
runtime_data <- html_text(html_nodes(html,'.text-muted .runtime'))
runtime_data = as.numeric(gsub(" min", "", runtime_data))
movies <- data.frame(rank = rank_data, title = title_data, runtime = runtime_data, stringsAsFactors = FALSE)
```
## Завдання 1
Виведіть перші 6 назв фільмів дата фрейму.
```r
head(movies, 6)$title
```
```
[1] "Той, хто біжить по лезу 2049" "Гра Моллі"                    "Тор: Раґнарок"               
[4] "Назви мене своїм ім'ям"       "Вбивство священного оленя"    "Воно"   
```
## Завдання 2
Виведіть всі назви фільмів с тривалістю більше 120 хв.
```r
subset(movies, runtime > 120)$title
```
```
 [1] "Той, хто біжить по лезу 2049"             "Гра Моллі"                               
 [3] "Тор: Раґнарок"                            "Назви мене своїм ім'ям"                  
 [5] "Вбивство священного оленя"                "Воно"                                    
 [7] "Вартові Галактики 2"                      "Людина-павук: Повернення додому"         
 [9] "Зоряні війни: Епізод 8 - Останні Джедаї"  "Красуня і Чудовисько"                    
[11] "Пірати Карибського моря: Помста Салазара" "Форма води"                              
[13] "Логан: Росомаха"                          "Трансформери: Останній лицар"            
[15] "Валеріан і місто тисячі планет"           "Чужий: Заповіт"                          
[17] "Диво-жінка"                               "Kingsman: Золоте кільце"                 
[19] "Джон Вік 2"                               "Мати!"                                   
[21] "Темні часи"                               "Примарна нитка"                          
[23] "Король Артур: Легенда меча"               "Сім сестер"                              
[25] "The Shack"                                "1+1: Нова історія"                       
[27] "Метелик"                                  "Форсаж 8"                                
[29] "Вороги"                                   "Війна за планету мавп"                   
[31] "Saban's Могутні рейнджери"                "Постріл в безодню"                       
[33] "Зменшення"                                "Ферма Мадбаунд"                          
[35] "Усі гроші світу" 
```
## Завдання 3
Скільки фільмів мають тривалість менше 100 хв.
```r
nrow(movies[movies$runtime < 100, ])
```
```
[1] 12
```