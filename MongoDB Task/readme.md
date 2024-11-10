
          --------------------------------- >  MongoDB TasK  < ------------------------------------


---- > Design database for Zen class programme ?

==== > REQUIREDMENT :
     
    1. users
    2. codekata
    3. attendance
    4. topics
    5. tasks
    6. company_drives
    7. mentors


==== > CREATING A COLLECTIONS FOR ABOVE ZEN CLASS TASK :

    1. USERS COLLECTION :
 
     INPUT:

          db.users.insertOne({"user_id": 10,
             "name": "John Doe",
             "email": "john@example.com",
             "joined_date": 2023-08-01,
             "codekata_score": 45,
             "attendance": ["2023-10-05", "2023-10-06"]});
   
     OUTPUT:
         
           {

            id: 672dbaa7079d1f89e7b08d29
            user_id: 10
            name: "John Doe"
            email: "john@example.com"
            joined_date: 2014
            codekata_score: 45
            attendance: Array (2)

           }



    2. CODEKATA COLLECTION :

     INPUT:

          db.codekata.insertOne({  "user_id":1,
             "problem_id": 112,
             "problem_title": "Arrays - Easy Level",
             "status": "solved",
             "solved_at": "2023-10-01"});
     
     OUTPUT:
            
            {

             id: 672dc39b079d1f89e7b08d2b
             user_id: 1
             problem_id: 112
             problem_title: "Arrays - Easy Level"
             status: "solved"
             solved_at: "2023-10-01"
            
            }

    3. ATTENDANCE COLLECTION :

     INPUT:

          db.attendance.insertOne({ "user_id":2 ,
             "date": "2023-10-15",
             "status": "absent",
             "tasks_submitted": false});
     
     OUTPUT:
        
            {

             id: 672dc5e7079d1f89e7b08d2c
             user_id: 2
             date: "2023-10-15"
             status: "absent"
             tasks_submitted: "false"
            
            }

    4. TOPIC COLLECTION :

     INPUT:

          db.topics.insertOne({"topic_id":3,
             "title": "MongoDB Basics",
             "created_at": "2023-10-05",
             "mentor_id": 113});          

     OUTPUT:

            {

             id: 672dc84b079d1f89e7b08d2d
             topic_id: 3
             title: "MongoDB Basics"
             created_at: "2023-10-05"
             mentor_id: 113
            
            }  

    5. TASK COLLECTION :
  
     INPUT:

             db.task.insertOne({"task_id": 4,
               "title": "Create a CRUD App with MongoDB",
               "due_date": "2023-10-20",
               "assigned_to": [22,23],
               "topic_id": 114});   
     
     OUTPUT:
     
            {

             id: 672dc99c079d1f89e7b08d2e
             task_id: 4
             title: "Create a CRUD App with MongoDB"
             due_date: "2023-10-20"
             assigned_to: Array (2)
             topic_id: 114

            }


    6. COMPANY_DRIVE COLLECTION :

     INPUT:

             db.company_drive.insertOne({ "drive_id": 5,
               "company_name": "TechCorp",
               "date": "2023-10-20",
               "students_appeared": ["33", "34"]});               
 
     OUTPUT:
     
            {
           
             id: 672dcb8f079d1f89e7b08d2f
             drive_id: 5
             company_name: "TechCorp"
             date: "2023-10-20"
             students_appeared: Array (2)

            }

    7. MENTORS COLLECTION :     
 
     INPUT:
                
                 db.mentors.insertOne({"mentor_id": 6,
                    "name": "Jane Smith",
                    "topic_ids": ["66", "56"],
                    "mentees_count": 16});      
         
     OUTPUT: 
     
            {
 
             id: 672dcd20079d1f89e7b08d30
             mentor_id: 6
             name: "Jane Smith"
             topic_ids: Array (2)
             mentees_count: 16

            }


   ------------------------------------- > TASK QUESTIONS TO WRITE < -----------------------------------------

  ====> TASK :


       1. Find all topics and tasks taught in the month of October ?
              
                // Find topics created in October....

                QUREY :
                 
                 db.topics.find({
                   "created_at": {
                   $gte:("2023-10-01T00:00:00Z"),
                   $lte:("2023-10-31T23:59:59Z")} });

                OUTPUT :  

                 {
                    id: ObjectId('672dc84b079d1f89e7b08d2d'),
                    topic_id: 3,
                    title: 'MongoDB Basics',
                    created_at: '2023-10-05',
                    mentor_id: 113
     
                 }


                // Find tasks due in October.....

                QUREY :

                 db.task.find({
                   "due_date": {
                    $gte:("2023-10-01T00:00:00Z"),
                    $lte:("2023-10-31T23:59:59Z")} });

                OUTPUT :

                {

                  id: ObjectId('672dc99c079d1f89e7b08d2e'),
                  task_id: 4,
                  title: 'Create a CRUD App with MongoDB',
                  due_date: '2023-10-20',
                  assigned_to: [22,23],
                  topic_id: 114
                
                }
               
       2. Find all the company drives which appeared between 15th Oct 2020 and 31st Oct 2020 ?

                QUREY :

                 // all company_drive.... ?

                      db.company_drive.find({});

                OUTPUT :

                       {

                         id: ObjectId('672dcb8f079d1f89e7b08d2f'),
                         drive_id: 5,
                         company_name: 'TechCorp',
                         date: '2023-10-20',
                         students_appeared: ['15-10-20','31-10-20']

                        }


                 // find which appeared between 15th Oct 2020 and 31st Oct 2020 ?

                QUREY :
                     
                      db.company_drive.find({"company_name":'TechCorp'},{"students_appeared":1},{"date":1});

                OUTPUT :

                        {

                          id: ObjectId('672dcb8f079d1f89e7b08d2f'),
                          students_appeared: ['15-10-20','31-10-20']

                        }

        3.  Find all the company drives and students who appeared for the placement ?

                QUREY :
                  
                         db.company_drive.aggregate([{$lookup: {
                            from: "users",
                            localField: "students_appeared",
                            foreignField: "_id",
                            as: "students_info"}},
                         {
                            $project: {
                            company_name: 1,
                            date: 1,
                            students_info: { name: 1, email: 1 } }} ]);
                           
                OUTPUT :

                        {

                          id: ObjectId('672dcb8f079d1f89e7b08d2f'),
                          company_name: 'TechCorp',
                          date: '2023-10-20',
                          students_info: []

                          }           

        4. Find the number of problems solved by the user in Codekata ?                

                QUREY :

                        db.codekata.aggregate([{
                           $match: { status: "solved" } },
                         {
                          $group: {
                          id: "$user_id",
                          problems_solved: { $sum: 1 }} }

                         ]);

                OUTPUT :

                        {

                          id: 1,
                          problems_solved: 1

                          }  


        5. Find all the mentors with more than 15 mentees ?


                QUREY :       

                        db.mentors.find({
                        "mentees_count": { $gt: 15 }
                        });


                OUTPUT :  

                        {

                          id: ObjectId('672dcd20079d1f89e7b08d30'),
                              mentor_id: 6,
                              name: 'Jane Smith',
                              topic_ids: ['66','56'],
                              mentees_count: 16
                        
                        } 


            ------------------------------------- > DONE THE ALL GIVEN TASK < -----------------------------------