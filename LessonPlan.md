# Backend 13.1 - Create DynamoDB Table through Java code

This lesson plan template is aimed at preparing instructors to deliver a high quality live session to learners who have already prepared to work on the project by setting up any prerequisites before coming to class.

## Prep List

- [ ] Watch the lesson plan overview video. ***Link to be included once lesson plan is complete***
- [ ] After watching the lesson overview, internalize lesson plan in full
- [ ] Set up your local machine to be prepared to teach the live session. You should also go through the Guided Practice videos for the first two modules of Sprint 13. That will help you significantly with this code along. Also, make sure you have AWS access.
- [ ] If you are running code in your live session, make sure it works and the solution code is accurate
- [ ] If your lesson plan includes technical steps, be sure to have walked through them first before teaching

## Lesson Plan

### 1. Engage Classroom (1-2 min.)

- ***Remind learners of classroom expectations during live Code-Along instruction:***
  - Web cameras need to be on to have the best learning experience possible
  - Talk about how we are going to be using specific communication tools like Slido, Zoom, etc. during the live session
  - If you need additional technical support that goes beyond this Code-Along, please use the Hub's knowledge base to start or schedule 1:1 support

- ***Pick one activity*** to help build an online community here at BloomTech and get learners excited for live instruction:
  - ***Icebreaker:*** Fun activity that helps learners get to know one another
  - ***Pulse Check:*** Pose a question for learners to gauge how they are feeling/get a pulse on how where they are at this point in the course
  - ***Do Now:*** Pose a question that involves no guidance from you. Used to activate students’ learning for the lesson, surface prior knowledge from pre-work, and familiarize students with today’s content
  - ***Celebrations:*** Share a learner celebration, job offer, remind students why they are putting in all this hard work


### 2. Getting Started (3-5 min.)

- State that the learners should have already setup their Discussion CLI starter repository. This is the same repository as the one used in Module 1 and 2 of Sprint 13. So, if learners have implemented along with Guided Practice then they can just use the same repository.
- Remind learners what they’ve worked on up until now to get to this point within the sprint
  - DynamoDB (annotate, load, read, write)
  - How to create db tables using AWS clouformation (using yaml file) and AWS web.
- Utilize a metaphor or an analogy to get learners excited to learn the key concepts
  - You would be working with database mostly on daily basis. Hence, it is very important to learn different aspects of it. In module 1, we learned how to create a DynamoDB table through AWS web interface and then how to do the same thing using AWS cloudformation service through the terminal.
- Highlight the key concepts that will be covered during the live session
  - AWS region change for DynamoDB from within the code.
  - Creating a new DynamoDB table from within the code.
- Explain how these concepts are designed with the sprint challenge in mind and how it’s critical to the job
  - All project use some or the other kind of data. Data is saved in the databases. In our sprint challenge project also we will be using DynamoDB tables just like you would do it in any project at work.  

### 3. What Will We Be Building In This Code-Along? (2-3 min.)

We will learn
  - how to change AWS region for DynamoDB in code.
  - how to create a DynamoDb table through Java code.

***Preview End Result:*** 
```
Image here
```

### 4. Let’s Build (30 min.)


- **Problem:** 
  - First, we want to make our Discussion CLI project point to an AWS region other than the default region. 
  - Secondly, we want to be able to create a database table in DynamoDB through Java code.

- **Solution:** 
  - Modify the DynamoDBClientProvider.
  - Add a new Java class to create the new table.

- **Build It:** 
  
   - **Step 1:**  Modify the DynamoDBClientProvider. In bd-13-1-codealong-DynamodbCreateTable-starter/src/com/amazon/ata/dynamodbannotationsloadsave/cli/DiscussionCliRunner.java file, 
     - search for the method getDynamoDBMapper().
     - Current code:
       ```
       private static DynamoDBMapper getDynamoDBMapper() {
        if (null == mapper) {
            mapper = new DynamoDBMapper(DynamoDbClientProvider.getDynamoDBClient());
        }
        return mapper;
       }
       ```
       
      - New code: specify the region (Regions.US_WEST_1) in the getDynamoDBClient(). We have been using US_WEST_2 as the default region so far. Try changing it to US_WEST_1
       ```
       private static DynamoDBMapper getDynamoDBMapper() {
        if (null == mapper) {
            mapper = new DynamoDBMapper(DynamoDbClientProvider.getDynamoDBClient(Regions.US_WEST_1));
        }
        return mapper;
       }
       ```
     - Now, go ahead and make sure you create the table "DynamoDbAnnotationsLoadSave-Members" in AWS DynamoDB in US_WEST_1 region. You can do it directly through the AWS web interface.
     - To test you can run the DiscussionCliRunner.java. When prompted, give a username that is not existing in the db table. (Since this is a new table, there should not be any items in the table yet.)
     - Go to DynamoDB and check the items in the table. You would notice that the new user was created in US_WEST_1 region instead of US_WEST_2 region.

   - **Step 2:**  Create a database table in DynamoDB through Java code.
     - Add a new Java file "CreateNewTable.java"
     - Add the following code and explain as per the instruction video.

     ```
       public class CreateNewTable {
        public static void main(String[] args) throws Exception {
          DynamoDB dynamoDB = new DynamoDB(DynamoDBClientProvider.REMOTE_CLIENT);

          String tableName = "Student";
          try {
            System.out.println("creating table");
            Table table = dynamoDB.createTable(tableName,
                    Arrays.asList(new KeySchemaElement("id", KeyType.HASH)),
                    Arrays.asList(new AttributeDefinition("id", ScalarAttributeType.S)),
                    new ProvisionedThroughput(10L, 10L));
            table.waitForActive();
          }
          catch (Exception e) {
            System.err.println("unable");
            System.err.println(e.getMessage());
          }
        }
      }
     ```
     

### 5. Wrap Up (7-10 min.)

- ***Answer questions*** in a question and answer format, time permitting
- ***Restate*** the key concepts and why the project was important to the job
Q&A for the whole project time permitting
- ***Review*** next steps in the learner journey as it relates to the sprint of learning.  Example:  If you are in Code-Along 1, you would encourage the learner to spend time with the next two learning modules before attending Code-Along 2 for this sprint
- ***Encourage*** learners to sign up for another live Code-Along or repeat this one if they want to review it again
