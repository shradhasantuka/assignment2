# assignment2
solution for programming assignment2
#Step - 1 

#Read the test and train measurement data.
measurement_train <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\train\\X_train.txt")
measurement_test <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\test\\X_test.txt")

#Merge the test and train measurement data
measurement <- rbind(measurement_train,measurement_test)

#Read the features data and assign the variable names to the merged measurement data
features <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\features.txt")
names(measurement) <- features$V2

#Read the train and test activity data, merge them and asssign column name
activity_train <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\train\\y_train.txt")
activity_test <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\test\\y_test.txt")
activity <- rbind(activity_train,activity_test)
names(activity) <- "activity"

#Read the train and test subject data, merge them and assign column name
subject_test <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\test\\subject_test.txt")
subject_train <- read.table("C:\\Users\\shradhasantuka\\Downloads\\UCI HAR Dataset\\train\\subject_train.txt")
subject <- rbind(subject_train,subject_test)
names(subject) <- "subject"

#merge the measurement, activity and subject data into one table i.e. the final table
merged_final <- cbind(subject,activity,measurement)

#Step- 2  
#Merged_final data frame has duplicate row names. The following code makes the variable names unique and assigns them to the merged_final table
valid_column_names <- make.names(names=names(merged_final), unique=TRUE, allow_ = TRUE)
names(merged_final) <- valid_column_names

# Selects columns whose column names have mean and std
extract_data <- select(merged_final,subject, activity,grep("\\.[Mm]ean\\.+|\\.[Ss]td\\.+", colnames(merged_final)))

#Step-3
#Replces the numeric value of activity to character

merged_final$activity[merged_final$activity == 1] <- "Walking"
merged_final$activity[merged_final$activity == 2] <- "Walking_Upstairs"
merged_final$activity[merged_final$activity == 3] <- "Walking_Downstairs"
merged_final$activity[merged_final$activity == 4] <- "Sitting"
merged_final$activity[merged_final$activity == 5] <- "Standing"
merged_final$activity[merged_final$activity == 6] <- "Laying"

#Step- 4
#Group the data frame by subject and activity and then find the mean

data <- melt(data=merged_final, id = c("subject","activity"),measure.vars = colnames(merged_final[,3:68]))
final_table <- dcast(data,subject+activity ~ variable, mean)

