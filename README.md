## Getting started


The fastest way to get started with Godspeed is by following the [Godspeed documentation](https://docs.mindgrep.com/).

Create a API backend for a restaurant app, using Godspeed Framework 1.2k.
which has below REST API’s:

Method	URL	Description
GET	/reataurant/:restaurantId	Fetch a restaurant by restaurantId
POST	/reataurant	Createa a new restaurant
PUT	/reataurant	Update an existing restaurant
DELETE	/restaurant/:restaurantId	Delete an existing restaurant
POST	/restaurant/search	Fetch restaurants of a particular city, and have Menu Items also in the response, If `couponCode` is provided, it should filter only those menu items which are for that code.
Populate database with atleast 5 restaurants

Some coupon codes are HUNGRY25, HUNGRY50

Steps
Install Godspeed CLI in your local machine. Here is a detailed guide 983.
Create a new godspeed project restaurant-app using Godspeed CLI, You can follow this 983, Select postgresas database. and then Open it in VSCode.
Inside VSCode click on Open in Container. Now you newly created project is running in dev container.
From the terminal, run godspeed build and then godspeed dev. Your documentation will be served in selected port [default 3000].
Copy below prisma schema in src/datasources/postgres.prisma. You can read about Prisma ORM and Schema 262.
generator client {
  provider = "prisma-client-js"
  output   = "./generated-clients/postgres"
  previewFeatures = ["metrics"]
}

datasource db {
  provider = "postgresql"
  url      = env("POSTGRES_URL")
}

model Owner {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
}

model Restaurant {
  id         Int        @id @default(autoincrement())
  createdAt  DateTime   @default(now())
  name      String
  since  DateTime
  isOpen  Boolean    @default(false)
  opsStartTime DateTime
  opsEndTime DateTime
  ownerId   Int?
  slug     String    @unique
  description String?
  location String
  menuItems MenuItems[]
}

model Category {
  id    Int    @id @default(autoincrement())
  name  String
}

model MenuItems {
  id  Int @id @default(autoincrement())
  name String
  description String?
  price Int
  couponCode String[]
  restaurant Restaurant @relation(fields: [restaurantId], references: id)
  restaurantId  Int
}

model Order {
  id Int @id @default(autoincrement())
  frmoRestaurant Int
  orderStatus OrderStatus @default(NOT_INITIATED)
  placedAt DateTime?
  fulfilledAt DateTime?
  orderItems OrderItem[]
}

model OrderItem {
  id Int @id @default(autoincrement())
  menuItemId Int
  quantity Int
  order Order @relation(fields: [orderId], references: id)
  orderId Int
}

enum OrderStatus {
  INITIATED
  NOT_INITIATED
  WAITING_FOR_APPROVAL_FROM_RESTAURANT
  WAITING_FOR_DELIVERY_PARTNER
  PLACED
  PICKUP_BY_DELIVERY_PARTNER
  DELIVERED
  READY_TO_PICKUP
}
You can use godspeed gen-crud-api to auto-generate REST api based on your schema.
For the last api in problem statement, These things you might helpful.
- Prisma findMany link 238
- Inline JS in Godspeed Workflow link 134
- Example of multiple task with arguments link 119
Evaluation
Your code is hosted in a public repository.
Your solution will be only evaluated if,
You follow the GodspeedSystems github org. link 259
You have starred, gs-node-service repository of Godspeed. link 123
References
Getting started guide 267
Documentation 1.2k
Demo video of the godspeed framework part one 287, part two 110.