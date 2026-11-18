# Pawfect

**Course:** Electronic Business Development (BINF 503) · Winter 2026
**Instructor:** Dr. Nourhan Hamdi
**Teaching Assistants:** Mr. Nour Gaser, Mr. Omar Alaa, Ms. Mariam Khalid, Mr. Younes Zied, Mr. Mahmoud Hani

## 1. Team

| Name | Student ID | Tutorial group | GitHub username |
|---|---|---|---|
| Jana Galal | 16001815 | T9 | janagalal12 |
| Joudy Ahmed | 16001356 | T6 | 1joudy |
| Norin Ashraf | 16007765 | T6 | norinashraf |
| Khaled Elmahdy | 14002824 | T9 | [username] |
| Marwan Shalaby | 13002013 | T6 | [username] |

## 2. The idea

Pawfect is a pet adoption web app for people in Egypt who want to rehome a pet or adopt one. Owners, rescuers and shelters post pets that need a home, with a photo and details, and people who want to adopt browse the pets and send an adoption request. Today this happens in scattered Facebook groups and WhatsApp chats, where posts get buried, the same pet is offered to many people at once, and nobody can tell whether a pet is still available. Pawfect keeps every pet and every adoption request in one place, so pets find homes faster and owners can choose the right adopter.

## 3. Features

### 3.1 The full product

- Register and log in with a name, email and password
- Post a pet with name, species, age, description, health notes and a photo
- Edit, delete and manage my own pet posts
- Browse all available pets on the home page
- Search pets by species and age, and filter by city
- Send an adoption request with a short message
- Withdraw my own adoption request
- Accept or reject adoption requests on my pets, which marks the pet as adopted
- Shelter profile pages with all their pets
- Save pets to a favourites list
- Lost and found pet board
- In-app chat between owner and adopter
- Email and in-app notifications for new and accepted requests
- Vaccination record upload for each pet
- Foster home sign-up for people who can host a pet temporarily
- Report a post, with an admin panel to review reports
- Mobile app for Android and iOS

### 3.2 The use cases we will build

| # | Use case | What the user does | What they see | What is stored |
|---|---|---|---|---|
| 1 | Registration and login | Registers with name, email and password; logs in with email and password | Their home page, with their name in the header | A User, with the password hashed |
| 2 | Post a pet for adoption | Fills in a form with name, species, age, description and an optional image link, then submits it | The new pet on its own page and in the list of available pets | A Pet linked to its owner, with status "available" |
| 3 | Manage my pets | Opens "My pets", edits a pet's details or deletes the post | The updated pet, or the list without the deleted pet | The Pet updated or deleted; only its owner can do this |
| 4 | Request to adopt | Opens a pet and sends an adoption request with a short message; can withdraw the request while it is still pending | Their request under "My requests", with status "pending" | An AdoptionRequest linked to the pet and the applicant; deleted if withdrawn |
| 5 | Respond to adoption requests | As the owner, opens the requests on one of their pets and accepts or rejects each one | The request's new status; after accepting, the pet shows "Adopted" | AdoptionRequest.status set to "accepted" or "rejected"; on accept, Pet.status set to "adopted" |

## 4. Who owns what

| Member | Use case |
|---|---|
| Jana Galal | 1. Registration and login |
| Khaled Elmahdy | 2. Post a pet for adoption |
| Marwan Shalaby | 3. Manage my pets |
| Joudy Ahmed | 4. Request to adopt |
| Norin Ashraf | 5. Respond to adoption requests |

## 5. Data model

### User

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }, // stored hashed, never as plain text
}, { timestamps: true });
```

### Pet

```js
const petSchema = new mongoose.Schema({
  name: { type: String, required: true },
  species: {
    type: String,
    enum: ['Cat', 'Dog', 'Bird', 'Rabbit', 'Other'],
    required: true,
  },
  ageYears: { type: Number, min: 0 },
  description: { type: String },
  imageUrl: { type: String },
  owner: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  status: { type: String, enum: ['available', 'adopted'], default: 'available' },
}, { timestamps: true });
```

### AdoptionRequest

```js
const adoptionRequestSchema = new mongoose.Schema({
  pet: { type: mongoose.Schema.Types.ObjectId, ref: 'Pet', required: true },
  applicant: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  message: { type: String },
  status: {
    type: String,
    enum: ['pending', 'accepted', 'rejected'],
    default: 'pending',
  },
}, { timestamps: true });
```

## 6. Run it locally

> Leave this section as it is until Submission 2 (Thursday 10 December 2026,
> 23:59). Then fill it in: we follow these steps to run your project.

### Back end

```bash
cd backend
npm install
# create backend/.env from backend/.env.example (see the table below)
npm start
```

The API runs on http://localhost:3000.

| Variable in `.env` | What it is |
|---|---|
| `MONGODB_URI` | The connection string of your MongoDB Atlas database |
| `JWT_SECRET` | Any long random text, used to sign login tokens |


### Front end

```bash
cd frontend
npm install
npm run dev
```

The app runs on [the address `npm run dev` prints, e.g. http://localhost:5173].
