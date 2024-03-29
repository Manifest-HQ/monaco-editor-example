<template>
  <div style="background-color: #202020; padding: 24px 16px">
    <MonacoEditor
      v-model="value"
      lang="prisma"
      :options="{
        theme: 'vs-dark'
      }"
      style="height: 100vh;" />
  </div>
</template>

<script setup>
onMounted(() => {
  const monaco = useMonaco()

  monaco.languages.register({ id: 'prisma' })
  monaco.languages.setMonarchTokensProvider('prisma', {
    tokenizer: {
      root: [
        // Functions like autoincrement(), now(), etc.
        [/\b(?:autoincrement|now|uuid|cuid|dbgenerated|env)\b\(\)/, 'function'],

        // Prisma-specific directives
        [/@\b(?:relation|id|default|unique|map|updatedAt)\b/, 'annotation'],
            
        // Built-in types and modifiers
        [/\b(?:Int|String|Boolean|DateTime|Float|Json|Bytes|BigInt|Decimal|enum|model)\b/, 'keyword'],
        [/!/, 'operator'], // Non-nullable fields
        
        // Fields and Types
        [/\b[A-Z][a-zA-Z0-9_]*\b/, 'type.identifier'], // Type names
        [/\b[a-z_][a-zA-Z0-9_]*\b(?=\s+\b(?:Int|String|Boolean|DateTime|Float|Json|Bytes|BigInt|Decimal)\b)/, 'variable'], // Field names

        // Field names that end with Id or IDs correctly like userpostId
        [/\b(?:\w*Id|\w*IDs)\b/, 'variable.id'], 

        // Comments
        [/#.*$/, 'comment'], // Hash style comments
        [/\/\/.*$/, 'comment'], // Double slash comments
        
        // Strings
        [/"(?:[^"\\]|\\.)*"/, 'string'], // Double quoted strings
        [/'(?:[^'\\]|\\.)*'/, 'string'], // Single quoted strings
        
        // Operators and Punctuation
        [/[\[\]{}().,;]+/, 'delimiter'], // Brackets, delimiters, semicolon
        
        // Annotations
        [/@\w+/, 'annotation'], // Other annotations like @map, @unique
        
        // Numeric Constants
        [/\b\d+\b/, 'number'],
      ]
    }
  })
})

const value = ref(`model User {
  id         Int       @id @default(autoincrement())
  email      String    @unique
  password   String
  name       String?
  posts      Post[]    @relation(name: "PostToUser")
  profile    Profile?  @relation(name: "ProfileToUser")
  comment    Comment[] @relation(name: "CommentToUser")
  NEW        Comment[] @relation(name: "CommentToUser1")
  new        String?
  new1       String?
  newww      String?
  ad         String?
  userpostId Int?
  userpost   userpost? @relation(name: "UserTouserpost", fields: [userpostId], references: [id])
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  userId Int     @unique
  user   User    @relation(name: "ProfileToUser", fields: [userId], references: [id])
}

model Post {
  id         Int              @id @default(autoincrement())
  title      String
  content    String?
  published  Boolean          @default(false)
  authorId   Int
  author     User             @relation(name: "PostToUser", fields: [authorId], references: [id])
  comments   Comment[]        @relation(name: "CommentToPost")
  categories CategoryToPost[] @relation(name: "CategoryToPostToPost")
  createdAt  DateTime         @default(now())
  updatedAt  DateTime         @updatedAt
  tags       Tag[]            @relation(name: "PostToTag")
}

model Comment {
  id        Int      @id @default(autoincrement())
  text      String
  postId    Int
  post      Post     @relation(name: "CommentToPost", fields: [postId], references: [id])
  authorId  Int      @unique
  author    User     @relation(name: "CommentToUser", fields: [authorId], references: [id])
  createdAt DateTime @default(now())
  userId    Int?
  user      User?    @relation(name: "CommentToUser1", fields: [userId], references: [id])
}

model Category {
  id    Int              @id @default(autoincrement())
  name  String           @unique
  posts CategoryToPost[] @relation(name: "CategoryToCategoryToPost")
}

model CategoryToPost {
  postId     Int
  categoryId Int
  post       Post     @relation(name: "CategoryToPostToPost", fields: [postId], references: [id])
  category   Category @relation(name: "CategoryToCategoryToPost", fields: [categoryId], references: [id])

  @@id([postId, categoryId])
}

model Tag {
  id     Int         @id @default(autoincrement())
  name   String      @unique
  posts  Post        @relation(name: "PostToTag", fields: [postId], references: [id])
  postId Int
  Cat    Categories?
  Ad     Categories  @default(Cat1)
}

model userpost {
  id     Int     @id @default(autoincrement())
  userId String?
  user   User[]  @relation(name: "UserTouserpost")
}

model s {
  id Int @id @default(autoincrement())
}

enum Categories {
  Cat1
  Cat2
}`)
</script>

<style lang="sass">
html, body
  margin: 0
</style>