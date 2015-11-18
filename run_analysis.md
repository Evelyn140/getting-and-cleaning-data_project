# getting-and-cleaning-data_project

rm(list=ls())

x_train <- read.table("UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("UCI HAR Dataset/train/Y_train.txt")
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt")

x_test <- read.table("UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("UCI HAR Dataset/test/Y_test.txt")
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")

# 1 merge
x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)

# 2, mean,std
features <- read.table("UCI HAR Dataset/features.txt")
meanstd <- grep("mean|std", features$V2)
x <- x[ , meanstd]

# 3 activity
activity <- read.table("UCI HAR Dataset/activity_labels.txt")
y$V1 <- activity[y$V1, 2]
names(y) <- "activity"

# 4 label data set
names(subject) <- "subject"
all <- cbind(x, y, subject)

# average
library(plyr)
aver <- ddply(all, .(subject, activity), function(x) colMeans(x[ , 1:79]))
write.table(aver, "average.txt", row.name = FALSE)
