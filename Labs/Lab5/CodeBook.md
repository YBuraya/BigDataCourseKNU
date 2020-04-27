# Лабораторна робота №5. CodeBook.
Зчитаємо всі необхідні дані, присвоюємо імена колонкам
```r
features <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/features.txt")
features_data <- features[[2]]
x_test <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/test/X_test.txt", col.names = features_data)
y_test <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/test/y_test.txt", col.names = c("Y"))
x_train <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/train/X_train.txt", col.names = features_data)
y_train <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/train/y_train.txt", col.names = c("Y"))
subject_test <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/test/subject_test.txt", col.names = c("Subject"))
subject_train <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/train/subject_train.txt", col.names = c("Subject"))
activity_labels <- read.table("C:/Users/Julia/Desktop/Lab5_Data/UCI HAR Dataset/activity_labels.txt")
```
Створюємо один набір даних, об'єднавши тестовий та тренувальний набори
```r
data_set_x <- rbind(x_train, x_test)
data_set_y <- rbind(y_train, y_test)
data_set_subject <- rbind(subject_test, subject_train)
```
Об'єднуємо всі дані в єдиний дата фрейм
```r
all_data <- cbind(data_set_x, data_set_y, data_set_subject)
```
Витягуємо лише вимірювання середнього значення та стандартного відхилення (mean and standard deviation)
```r
filtered_x <- select(select(data_set_x,  matches("mean|std")), !matches("Freq"))
```
Використовуємо описові назви діяльностей (activity) для найменування діяльностей у наборі даних та присвоюємо змінним у наборі даних описові імена.
```r
y_factor <- factor(data_set_y$Y)
levels(y_factor) <- activity_labels[,2]
data_set_y$Y <- y_factor
```
З набору даних з кроку 4 створити другий незалежний акуратний набір даних (tidy dataset) із середнім значенням для кожної змінної для кожної діяльності та кожного суб’єкту (subject).
```r
filtered_data <- cbind(data_set_y, data_set_subject, filtered_x)
tidy_data <- aggregate(.~Subject+Y, filtered_data, mean)
```
Зберігаємо трансформований набір даних
```r
write.table(tidy_data, "tidy_dataset.txt", row.names=FALSE)
```