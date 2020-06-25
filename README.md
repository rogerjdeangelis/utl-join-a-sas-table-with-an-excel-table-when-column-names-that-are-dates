# utl-join-a-sas-table-with-an-excel-table-when-column-names-that-are-dates
Join a sas table with an excel table when column names that are dates 
    Join a sas table with an excel table when column names that are dates                                                                     
                                                                                                                                              
       Explanation                                                                                                                            
                                                                                                                                              
            1. Create and excel sheet with column names that are dates.                                                                       
               Even though the format of the dates will be general excel considers them numeric dates                                         
            2. Transpose the dates to columns in the excel table.                                                                             
            3. Create a SAS table with some matches with the excel table                                                                      
            4. Join the SAS table with the excel sheet  on Term and date and add the age variable.                                            
                                                                                                                                              
    SAS Forum                                                                                                                                 
    https://tinyurl.com/ybpocdbw                                                                                                              
    https://communities.sas.com/t5/SAS-Programming/Look-Up-Table-from-Excel-File/m-p/665040                                                   
                                                                                                                                              
    github                                                                                                                                    
    https://tinyurl.com/yaafguzt                                                                                                              
    https://github.com/rogerjdeangelis/utl-join-a-sas-table-with-an-excel-table-when-column-names-that-are-dates                              
                                                                                                                                              
    * how to generalize;                                                                                                                      
    github                                                                                                                                    
    https://tinyurl.com/y99s3k7k                                                                                                              
    https://github.com/rogerjdeangelis/utl-import-excel-when-column-names-are-excel-dates                                                     
                                                                                                                                              
    * you can macrotize the dates using pure macro or macro arrays (array and do_over);                                                       
                                                                                                                                              
    /*                   _                                                                                                                    
    (_)_ __  _ __  _   _| |_                                                                                                                  
    | | `_ \| `_ \| | | | __|                                                                                                                 
    | | | | | |_) | |_| | |_                                                                                                                  
    |_|_| |_| .__/ \__,_|\__|                                                                                                                 
            |_|        _                                                                                                                      
      _____  _____ ___| |                                                                                                                     
     / _ \ \/ / __/ _ \ |                                                                                                                     
    |  __/>  < (_|  __/ |                                                                                                                     
     \___/_/\_\___\___|_|                                                                                                                     
                                                                                                                                              
    */                                                                                                                                        
                                                                                                                                              
    options validvarname=any;                                                                                                                 
                                                                                                                                              
    %utlfkil(m:/xls/havXls.xlsx); * delete workbook if exists;                                                                                
                                                                                                                                              
    libname xel "m:/xls/havXls.xlsx";                                                                                                         
                                                                                                                                              
    * create table have in have sheet inside workbook "m:/xls/have.xlsx";                                                                     
                                                                                                                                              
    data xel.havXls;                                                                                                                          
     input TERM '6/1/2020'n '6/2/2020'n '6/3/2020'n '6/4/2020'n;                                                                              
    cards4;                                                                                                                                   
    1 10 11 12 13                                                                                                                             
    2 21 22 23 24                                                                                                                             
    3 31 32 33 34                                                                                                                             
    4 41 42 43 44                                                                                                                             
    ;;;;                                                                                                                                      
    run;quit;                                                                                                                                 
                                                                                                                                              
    libname xel clear;                                                                                                                        
                                                                                                                                              
    options validvarname=upcase;                                                                                                              
                                                                                                                                              
    m:/xls/have.xlsx  (sheetname HAVE abd Namrd rang HAVE)                                                                                    
                                                                                                                                              
       -------------------------------------------------------                                                                                
     1 |     TERM|  6/1/2010|  6/1/2010|   6/1/2010  6/1/2010|                                                                                
       |-----------------------------------------------------|                                                                                
     2 |        1|        10|        11|        12|        13|                                                                                
       |---------+----------+----------+----------+----------|                                                                                
     3 |        2|        21|        22|        23|        24|                                                                                
       |---------+----------+----------+----------+----------|                                                                                
     4 |        3|        31|        32|        33|        34|                                                                                
       |---------+----------+----------+----------+----------|                                                                                
     5 |        4|        41|        42|        43|        44|                                                                                
       -------------------------------------------------------                                                                                
                                                                                                                                              
     [HAVE]                                                                                                                                   
                                                                                                                                              
    /*                                                                                                                                        
     ___  __ _ ___                                                                                                                            
    / __|/ _` / __|                                                                                                                           
    \__ \ (_| \__ \                                                                                                                           
    |___/\__,_|___/                                                                                                                           
                                                                                                                                              
    */                                                                                                                                        
                                                                                                                                              
    data havSas;                                                                                                                              
     input term var$ val age;                                                                                                                 
    cards4;                                                                                                                                   
    1 6/1/2020 10 80                                                                                                                          
    2 6/2/2020 22 71                                                                                                                          
    3 6/3/2020 33 62                                                                                                                          
    4 6/4/2020 44 15                                                                                                                          
    ;;;;                                                                                                                                      
    run;quit;                                                                                                                                 
                                                                                                                                              
    Up to 40 obs WORK.HAVSAS total obs=4                                                                                                      
                                                                                                                                              
     TERM      VAR       VAL    AGE                                                                                                           
                                                                                                                                              
       1     6/1/2020     10     80                                                                                                           
       2     6/2/2020     22     71                                                                                                           
       3     6/3/2020     33     62                                                                                                           
       4     6/4/2020     44     15                                                                                                           
                                                                                                                                              
    /*          _                                                                                                                             
     _ __ _   _| | ___  ___                                                                                                                   
    | `__| | | | |/ _ \/ __|                                                                                                                  
    | |  | |_| | |  __/\__ \                                                                                                                  
    |_|   \__,_|_|\___||___/                                                                                                                  
                                                                                                                                              
    */                                                                                                                                        
                                                                                                                                              
    Join  havXls with HAVSas on havXls.term = havSas.term and havXls.var = havSas.ver                                                         
                                                                                                                                              
     Note                                                                                                                                     
                                                                                                                                              
        TERM      VAR       VAL    AGE     -------------------------------------------------------                                            
                                         1 |     TERM|  6/1/2010|  6/1/2010|   6/1/2010  6/1/2010|                                            
          1     6/1/2020     10     80     |-----------------------------------------------------|                                            
                                         2 |        1|        10|        11|        12|        13|                                            
                                           -------------------------------------------------------                                            
                                                                                                                                              
        Matches                                                                                                                               
                                                                                                                                              
          havXls.term = havSas.term and havXls.var = havSas.ver                                                                               
              1       =    1               10      =    10                                                                                    
                                                                                                                                              
    /*           _               _                                                                                                            
      ___  _   _| |_ _ __  _   _| |_                                                                                                          
     / _ \| | | | __| `_ \| | | | __|                                                                                                         
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                          
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                         
                    |_|                                                                                                                       
    */                                                                                                                                        
                                                                                                                                              
     WORK.WANT total obs=4                                                                                                                    
                                                                                                                                              
     TERM      VAR       VAL    AGE                                                                                                           
                                                                                                                                              
       1     6/1/2020     10     80                                                                                                           
       2     6/2/2020     22     71                                                                                                           
       3     6/3/2020     33     62                                                                                                           
       4     6/4/2020     44     15                                                                                                           
                                                                                                                                              
    /*                                                                                                                                        
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                        
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                       
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                       
    | .__/|_|  \___/ \___\___||___/___/                                                                                                       
    |_|                                                                                                                                       
    */                                                                                                                                        
                                                                                                                                              
    options validvarname=upcase;                                                                                                              
                                                                                                                                              
    * transpose excel;                                                                                                                        
    * this can eas                                                                                                                            
    proc sql dquote=ansi;                                                                                                                     
         connect to excel                                                                                                                     
            (Path="m:/xls/havXls.xlsx" );                                                                                                     
            create                                                                                                                            
                table havXls as                                                                                                               
            select                                                                                                                            
                *                                                                                                                             
                from connection to Excel                                                                                                      
                (                                                                                                                             
                  Select term ,'6/1/2020' as var,[6/1/2020] as val from [havXls] union                                                        
                  Select term ,'6/2/2020' as var,[6/2/2020] as val from [havXls] union                                                        
                  Select term ,'6/3/2020' as var,[6/3/2020] as val from [havXls] union                                                        
                  Select term ,'6/4/2020' as var,[6/4/2020] as val from [havXls]                                                              
                );                                                                                                                            
            disconnect from Excel;                                                                                                            
     Quit;                                                                                                                                    
                                                                                                                                              
    /*                                                                                                                                        
                                                                                                                                              
    Use SQL to transpose                                                                                                                      
                                                                                                                                              
    WORK.HAVXLS total obs=16                                                                                                                  
                                                                                                                                              
     TERM      VAR       VAL                                                                                                                  
                                                                                                                                              
       1     6/1/2020     10  **match                                                                                                         
       1     6/2/2020     11                                                                                                                  
       1     6/3/2020     12                                                                                                                  
       1     6/4/2020     13                                                                                                                  
       2     6/1/2020     21                                                                                                                  
       2     6/2/2020     22  ** matck                                                                                                        
       2     6/3/2020     23                                                                                                                  
       2     6/4/2020     24                                                                                                                  
       3     6/1/2020     31                                                                                                                  
       3     6/2/2020     32                                                                                                                  
       3     6/3/2020     33  ** match                                                                                                        
       3     6/4/2020     34                                                                                                                  
       4     6/1/2020     41                                                                                                                  
       4     6/2/2020     42                                                                                                                  
       4     6/3/2020     43                                                                                                                  
       4     6/4/2020     44  **match                                                                                                         
    */                                                                                                                                        
                                                                                                                                              
                                                                                                                                              
    proc sql;                                                                                                                                 
      create                                                                                                                                  
         table want as                                                                                                                        
      select                                                                                                                                  
         l.term                                                                                                                               
        ,l.var                                                                                                                                
        ,l.val                                                                                                                                
        ,l.age                                                                                                                                
      from                                                                                                                                    
         havSas as l, havXls as r                                                                                                             
      where                                                                                                                                   
         l.term = r.term and                                                                                                                  
         l.var =r.var                                                                                                                         
    ;quit;                                                                                                                                    
                                                                                                                                              
                                                                                                                                              
    WORK.WANT total obs=4                                                                                                                     
                                                                                                                                              
     TERM      VAR       VAL    AGE                                                                                                           
                                                                                                                                              
       1     6/1/2020     10     80                                                                                                           
       2     6/2/2020     22     71                                                                                                           
       3     6/3/2020     33     62                                                                                                           
       4     6/4/2020     44     15                                                                                                           
                                                                                                                                              
                                                                                                                                              
