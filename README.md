# IBM_R_Week2_Assignment1library(tidyverse)
airline = read.csv(choose.files(),header=TRUE)

##DataProcessing##

head(airline)

airline%>% 
        summarize(count = sum(is.na(CarrierDelay)))

airline%>% 
        map(~sum(is.na(.))) ## Missing_Value in each column

                            ## ~ seperate the right from the left. 


drop_na_cols = airline%>% 
              select(-c("DivDistance","DivArrDelay"))
dim(airline)
dim(drop_na_cols)

drop_na_only = airline%>% 
                drop_na(CarrierDelay)
drop_na_only$CarrierDelay
anyNA(drop_na_only[["CarrierDelay"]])

replace_na = drop_na_only%>%  
                        replace_na(list(CarrierDelay = 0,
                                        WeatherDelay = 0,
                                        NASDelay = 0,
                                        SecurityDelay = 0,
                                        LateAircraftDelay = 0))
replace_na

airline$CarrierDelay

replace_mean = airline%>%
                replace_na(list(CarrierDelay = mean(airline$CarrierDelay,na.rm=TRUE)))


airline%>% 
        summarize_all(class)%>%
          gather(variable,class)
glimpse(airline)
airline%>% 
          summarize_all(class)%>% 
            gather(varible,class)

date_airline = airline%>% 
            separate(FlightDate,sep="-", into = c('Year','Month','Day'))
convert_date_airline = date_airline %>%
            select(Year,Month,Day)%>% 
            mutate_all(type.convert)%>%
            mutate_if(is.character,as.numeric)
convert_date_airline%>% 
            summarize_all(class)%>%
            gather(variable,class)
convert_date_airline
