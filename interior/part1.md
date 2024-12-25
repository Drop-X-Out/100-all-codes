# Interio - Full Stack

---

# Introduction

- Interio is a platform for interior designers to share work, get inspiration, and showcase designs
- Key features include user registration, design posting with images, tag-based design browsing, and personal collections
- The app structure includes pages for home, designs, profile, and shot uploads
- Server actions handle vendor and shot-related operations, including authentication and CRUD functions
- The frontend UI is responsive and includes features like design browsing, profile editing, and shot uploads
- User profiles display work history, skills, and design collections
- The project emphasizes a clean, modern design with a focus on showcasing interior design work

# Technologies Used

- Nextjs 14 (App Router)
- Tailwind CSS with shadcn/ui
- Server Actions
- TypeScript
- React Hook Forms
- Uploadthing
- MongoDB
- Uploadthing

---

# Project Setup

- Create a new directory for the project.
- Sign up to the uploadthing website and get the UPLOADTHING_SECRET and UPLOADTHING_APP_ID.
- Paste the below command to initialize a nextjs project. (you may also use npm/yarn/bun)
    
    ```powershell
    pnpx create-next-app@latest
    ```
    
- Exchange the pakage.json file to install all dependencies.
    
    ```jsx
    {
      "name": "interio",
      "version": "0.1.0",
      "private": true,
      "scripts": {
        "dev": "next dev",
        "build": "next build",
        "start": "next start",
        "lint": "next lint",
        "format": "prettier --write \"**/*.{js,jsx,ts,tsx,json,css,scss,md}\""
      },
      "dependencies": {
        "@hookform/resolvers": "^3.9.0",
        "@portabletext/react": "^3.1.0",
        "@radix-ui/react-alert-dialog": "^1.1.1",
        "@radix-ui/react-checkbox": "^1.1.1",
        "@radix-ui/react-dialog": "^1.1.1",
        "@radix-ui/react-label": "^2.1.0",
        "@radix-ui/react-separator": "^1.1.0",
        "@radix-ui/react-slot": "^1.1.0",
        "@reduxjs/toolkit": "^2.2.7",
        "@sanity/client": "^6.21.1",
        "@sanity/image-url": "^1.0.2",
        "@sanity/vision": "^3.53.0",
        "@types/node": "22.1.0",
        "@types/react": "18.3.3",
        "@types/react-dom": "18.3.0",
        "@uploadthing/react": "^6.7.2",
        "autoprefixer": "10.4.20",
        "axios": "^1.7.3",
        "bcryptjs": "^2.4.3",
        "class-variance-authority": "^0.7.0",
        "clsx": "^2.1.1",
        "eslint": "9.8.0",
        "eslint-config-next": "14.2.5",
        "eslint-plugin-unicorn": "^55.0.0",
        "framer-motion": "^11.3.24",
        "joi": "^17.13.3",
        "jsonwebtoken": "^9.0.2",
        "lucide-react": "^0.426.0",
        "mongoose": "^8.5.2",
        "next": "14.2.5",
        "next-themes": "^0.3.0",
        "postcss": "8.4.41",
        "react": "18.3.1",
        "react-dom": "18.3.1",
        "react-hook-form": "^7.52.2",
        "react-icons": "^5.2.1",
        "react-redux": "^9.1.2",
        "sanity": "^3.53.0",
        "styled-components": "^6.1.12",
        "tailwind-merge": "^2.4.0",
        "tailwind-scrollbar": "^3.1.0",
        "tailwindcss": "3.4.9",
        "tailwindcss-animate": "^1.0.7",
        "typescript": "5.5.4",
        "uploadthing": "^6.13.2",
        "vaul": "^0.9.1"
      },
      "devDependencies": {
        "@ianvs/prettier-plugin-sort-imports": "^4.3.1",
        "@tailwindcss/typography": "^0.5.14",
        "@types/bcryptjs": "^2.4.6",
        "@types/jsonwebtoken": "^9.0.6",
        "@typescript-eslint/eslint-plugin": "^8.0.1",
        "eslint-config-prettier": "^9.1.0",
        "prettier": "^3.3.3",
        "prettier-plugin-tailwindcss": "^0.6.5"
      }
    }
    
    ```
    
- App Structure
    
    ```jsx
    ├── app
    │   ├── layout.tsx
    │   ├── page.tsx
    │   ├── shots
    │   │   └── page.tsx
    │   ├── profile
    │   │   └── page.tsx
    ├── components
    │   ├── ui
    │   │   ├── alert-dialog.tsx
    │   │   ├── button.tsx
    │   │   ├── dropdown-menu.tsx
    │   │   └── ...
    │   ├── main-nav.tsx
    │   ├── page-header.tsx
    │   └── ...
    ├── context
    │   ├── hook.ts
    │   ├── store.ts
    │   └── theme.ts
    ├── lib
    │   ├── utils.ts
    │   ├── actions
    │   │   ├── shot.action.ts
    │   │   └── vendor.action.ts
    │   ├── models
    │   │		├── collection.model.ts
    │   │   ├── shot.model.ts
    │   │   └── vendor.model.ts
    │   └── db.ts
    ├── styles
    │   └── globals.css
    ├── next.config.js
    ├── package.json
    ├── tailwind.config.js
    └── tsconfig.json
    ```
    
- Environment Variables (.env)
    
    ```powershell
    JWT_SECRET=
    MONGODB_URL=
    UPLOADTHING_SECRET=
    UPLOADTHING_APP_ID=
    ```
    

## Some utility functions

Below are some utility functions that will assist in various tasks across the project:

```jsx
import jwt, { JwtPayload } from "jsonwebtoken"
import { generateComponents } from "@uploadthing/react"
import type { OurFileRouter } from "../app/api/uploadthing/core"

const JWT_SECRET = process.env.JWT_SECRET

const getIdByToken = async (token: any) => {
  try {
    if (!JWT_SECRET) {
      throw new Error("JWT_SECRET is not defined")
    }
    const decoded = jwt.verify(token.value, JWT_SECRET) as string | JwtPayload
    if (typeof decoded === "string") {
      throw new Error("Invalid token")
    }
    return decoded._id
  } catch (error) {
    throw new Error(`No token : ${error}`)
  }
}

export default getIdByToken

export function getErrorMessage(error: unknown): string {
  let message: string
  if (error instanceof Error) {
    message = error.message
  } else if (error && typeof error === "object" && "message" in error) {
    message = String(error.message)
  } else if (typeof error === "string") {
    message = error
  } else {
    message = "Something went wrong"
  }
  return message
}

export function formatDateInDMY(inputDate: string): string {
  const date = new Date(inputDate)
  if (isNaN(date.getTime())) return ""

  const options: Intl.DateTimeFormatOptions = {
    day: "2-digit",
    month: "short",
    year: "numeric",
  }
  return date.toLocaleDateString(undefined, options)
}

export function formatDMY(date: string, weekday?: boolean) {
  const options: {
    year: string
    month: string
    day: string
    weekday?: string
  } = { year: "numeric", month: "long", day: "numeric" }
  if (weekday) {
    options.weekday = "short"
  }
  // @ts-ignore -d s
  return new Date(date).toLocaleDateString(undefined, options)
}

export const { UploadButton, UploadDropzone, Uploader } = generateComponents<OurFileRouter>()
```

## Types

Types used across the project.

```jsx
export type OwnerType = {
  name: string
  follower: string[]
  following: string[]
  likedshot?: string[]
  email: string
  _id: string
}

export type shotData = {
  title: string
  category: string
  description: string
  tags: string[]
  images: { title: string; url: string; _id: string }[]
  _id: string
  owner: OwnerType
}

export type shotDataArr = shotData[]

export type messageProp = {
  sender: string
  content: string
  readBy: string[]
  createdAt: string
}

export type vendorType = {
  _id: string
  name: string
  follower: string[]
  following: string[]
  likedShot: string[]
}

export type reduxVendor = {
  vendor: string
  v_id: string
  token: string
}

```

## APIs Setup

1. MongoDB connection in the nextjs environment.
    
    ```powershell
    import mongoose from "mongoose"
    
    let isConnected = false // Variable to track the connection status
    
    export const connectToDB = async () => {
      // Set strict query mode for Mongoose to prevent unknown field queries.
      mongoose.set("strictQuery", true)
    
      if (!process.env.MONGODB_URL) return console.log("Missing MongoDB URL")
    
      // If the connection is already established, return without creating a new connection.
      if (isConnected) {
        console.log("MongoDB connection already established")
        return
      }
    
      try {
        await mongoose.connect(process.env.MONGODB_URL)
    
        isConnected = true // Set the connection status to true
        console.log("MongoDB connected")
      } catch (error) {
        console.log(error)
      }
    }
    
    ```
    

1. Database Schema Design
    - Vendor
        
        ```jsx
        import bcrypt from "bcryptjs"
        import jwt from "jsonwebtoken"
        import mongoose from "mongoose"
        
        import { IVendor, IVendorModel } from "./types/vendor-type"
        
        mongoose.set("strictQuery", true)
        
        const vendorSchema = new mongoose.Schema<IVendor, IVendorModel>(
          {
            type: {
              type: String,
              default: "vendor",
            },
            socketId: {
              type: String,
            },
            name: {
              type: String,
              trim: true,
              required: true,
            },
            profilePic: {
              type: String,
              default: null,
            },
            username: {
              type: String,
              trim: true,
              required: true,
              unique: true,
            },
            email: {
              type: String,
              unique: true,
              trim: true,
              required: true,
            },
            contact: {
              type: Number,
            },
            password: {
              type: String,
              trim: true,
              required: true,
            },
            address: {
              type: String,
              trim: true,
              default: null,
            },
            biography: {
              type: String,
              trim: true,
              default: null,
            },
            workHistory: [
              {
                title: String,
                company: String,
                location: String,
                from: String,
                to: String,
              },
            ],
            lookingFor: [
              {
                title: String,
                location: String,
              },
            ],
            ownShot: [mongoose.Schema.Types.ObjectId],
            likedShot: [mongoose.Schema.Types.ObjectId],
            shotCollections: [mongoose.Schema.Types.ObjectId],
            socialLinks: [
              {
                platform: String,
                url: String,
              },
            ],
            skills: [String],
            follower: [String],
            following: [String],
            resetToken: String,
            expireToken: Date,
            otp: {
              type: String,
              default: null,
            },
            otpExpireIn: {
              type: Date,
              expireAfterSeconds: 150,
              default: null,
            },
            tokens: [
              {
                token: {
                  type: String,
                  required: true,
                },
              },
            ],
          },
          {
            timestamps: true,
          }
        )
        
        vendorSchema.virtual("Shot", {
          ref: "shotModel",
          localField: "_id",
          foreignField: "owner",
        })
        
        vendorSchema.methods.toJSON = function () {
          const vendor = this
          const vendorObject = vendor.toObject()
        
          delete vendorObject.password
          delete vendorObject.tokens
          delete vendorObject.otp
          return vendorObject
        }
        
        vendorSchema.methods.generateAuthToken = async function () {
          const vendor = this
          const JWT_SECRET = process.env.JWT_SECRET || null
          if (!JWT_SECRET) {
            throw new Error("JWT secret not found")
          }
          const token = jwt.sign({ _id: vendor._id.toString() }, JWT_SECRET, {
            expiresIn: "1h",
          })
          vendor.tokens = vendor.tokens.concat({ token })
          await vendor.save()
          return token
        }
        
        vendorSchema.statics.findByCredentials = async (email, password) => {
          const vendor = await Vendor.findOne({
            email,
          })
          if (!vendor) {
            throw new Error("Invalid vendor name or Sign up first !!")
          }
          const isMatch = await bcrypt.compare(password, vendor.password)
          if (!isMatch) {
            throw new Error("Invalid Password!")
          }
          return vendor
        }
        
        vendorSchema.pre("save", async function (next) {
          const vendor = this
          if (vendor.isModified("password")) {
            vendor.password = await bcrypt.hash(vendor.password, 8)
          }
          next()
        })
        
        const Vendor: IVendorModel = (mongoose.models.Vendor as IVendorModel) || mongoose.model<IVendor, IVendorModel>("Vendor", vendorSchema)
        
        export default Vendor
        
        ```
        
    - Shots
        
        ```jsx
        import mongoose from "mongoose"
        
        mongoose.set("strictQuery", true)
        
        const shotSchema = new mongoose.Schema(
          {
            title: {
              type: String,
              trim: true,
              required: true,
            },
            category: {
              type: String,
              trim: true,
              required: true,
              lowercase: true,
              index: true,
            },
            description: {
              type: String,
              required: true,
              trim: true,
            },
            tags: {
              type: [String],
              lowercase: true,
            },
            images: [
              {
                title: String,
                url: String,
              },
            ],
            owner: {
              type: String,
              requied: true,
              ref: "Vendor",
            },
            like: [mongoose.Schema.Types.ObjectId],
            comments: [
              {
                v_id: String,
                comment: String,
              },
            ],
            view: {
              type: Number,
              default: 0,
            },
          },
          {
            timestamps: true,
          }
        )
        
        const Shot = mongoose.models.Shot || mongoose.model("Shot", shotSchema)
        
        export default Shot
        
        ```
        
    - Collection
        
        ```jsx
        import mongoose from "mongoose"
        
        mongoose.set("strictQuery", true)
        
        const collectionSchema = new mongoose.Schema(
          {
            cname: {
              type: String,
              trim: true,
              required: true,
            },
            shots: [mongoose.Schema.Types.ObjectId],
            owner: {
              type: String,
              requied: true,
              ref: "Vendor",
            },
          },
          {
            timestamps: true,
          }
        )
        
        const Collection = mongoose.models.Collection || mongoose.model("Collection", collectionSchema)
        
        export default Collection
        
        ```
        
2. Server Actions
    - Vendor Actions
        
        ```jsx
        "use server"
        
        import { cookies } from "next/headers"
        import getIdByToken, { getErrorMessage } from "@/utils/helper"
        
        import SHOT from "../model/shot.model"
        import VENDOR from "../model/vendor.model"
        import { connectToDB } from "../mongoose"
        
        //-------------GET ALL VENDOR-------------//
        export const getAllVendors = async () => {
          try {
            connectToDB()
            const vendor = await VENDOR.find({})
            return { vendor }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------NEW VENDOR-------------//
        export const addVendor = async (body: any) => {
          try {
            connectToDB()
            body["username"] = body.email.split("@")[0]
            const vendor = new VENDOR(body)
            await vendor.save()
            const token = await vendor.generateAuthToken()
            cookies().set("token", token)
        
            return { name: vendor?.name, _id: vendor._id.toString(), token }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------LOGIN VENDOR-------------//
        export const vendorLogin = async (body: any) => {
          try {
            connectToDB()
            const vendor = await VENDOR.findByCredentials(body.email, body.password)
            if (!vendor) {
              throw new Error("Invalid Attempt, vendor not Found!")
            }
            const token = await vendor.generateAuthToken()
            cookies().set("token", token)
            return { name: vendor?.name, _id: vendor?._id.toString(), profilePic: vendor.profilePic, token }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------LOGOUT VENDOR-------------//
        export const vendorLogout = async () => {
          try {
            const cookieStore = cookies()
            const token = cookieStore.get("token")
            const _id = await getIdByToken(token)
        
            if (!_id) {
              return { message: "Token Expired", code: 500 }
            }
            const vendor = await VENDOR.findOne({ _id })
            if (!vendor) {
              throw new Error("Invalid Attempt, vendor not Found!")
            }
            cookies().delete("token")
            vendor.tokens = vendor.tokens.filter((tok) => {
              return tok.token !== token?.value
            })
        
            await vendor.save()
            return { message: "Logged Out!" }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------REVALIDATE VENDOR-------------//
        export const revalidateVendor = async () => {
          try {
            connectToDB()
        
            const cookieStore = cookies()
            const prevToken = cookieStore.get("token")
            if (!prevToken) {
              return { message: "Token Not Found", code: 500 }
            }
        
            const _id = await getIdByToken(prevToken)
            if (!_id) {
              return { message: "Token Expired", code: 500 }
            }
        
            const vendor = await VENDOR.findOne({ _id }).select("name profilePic tokens")
            if (!vendor) {
              return { message: "Vendor Not Found", code: 500 }
            }
            vendor.tokens = vendor.tokens.filter((tok) => {
              return tok.token !== prevToken?.value
            })
        
            const token = await vendor.generateAuthToken()
            await vendor.save()
            cookies().set("token", token)
            return { name: vendor?.name, _id: vendor._id.toString(), profilePic: vendor.profilePic, token, code: 200 }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------GET COMPLETE VENDOR-------------//
        export const getVendorProfile = async () => {
          try {
            connectToDB()
            const cookieStore = cookies()
            const token = cookieStore.get("token")
            if (!token) throw new Error("Token Not Found")
            const _id = await getIdByToken(token)
            if (!_id) throw new Error("Token Expired")
            const vendor = await VENDOR.findOne({ _id }).select("-password -tokens")
            if (!vendor) {
              throw Error("NO vendor data FOUND")
            }
            return { vendor }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------GET VENDOR TABS-------------//
        export const getTabs = async (tab: string) => {
          try {
            connectToDB()
            const cookieStore = cookies()
            const token = cookieStore.get("token")
            if (!token) return
            const _id = await getIdByToken(token)
            if (!_id) return
            let selectWord = "shotCollections"
            let tabArr: string | any[] = []
            if (tab === "liked-shot") {
              selectWord = "likedShot"
            }
            if (tab === "work") {
              selectWord = "ownShot"
            }
        
            const vdr = await VENDOR.findOne({ _id }).select(selectWord)
            if (!vdr) {
              throw Error("NO vendor data FOUND")
            }
        
            if (selectWord === "shotCollections") {
              // const collection = await COLLECTION.findById(vendor.shotCollections[0]);
              // tabArr = collection.shots;
            } else if (selectWord === "likedShot") {
              tabArr = vdr.likedShot
            } else {
              tabArr = vdr.ownShot
            }
            if (tabArr.length == 0) {
              return { shots: [] }
            } else {
              const shots = await SHOT.find({ _id: { $in: tabArr } })
              if (!shots) {
                throw Error("Unable to get shots!")
              }
              return { shots }
            }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------UPDATE VENDOR-------------//
        
        export const updateVendor = async (body: any) => {
          try {
            connectToDB()
            const cookieStore = cookies()
            const token = cookieStore.get("token")
            if (!token) return
            const _id = await getIdByToken(token)
            if (!_id) return
            const vendor: any = await VENDOR.findOne({ _id })
            if (!vendor) {
              throw Error("NO vendor data FOUND")
            }
            const updates = Object.keys(body)
            updates.forEach((update) => {
              vendor[update] = body[update]
            })
            await vendor.save()
            return { vendor }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        ```
        
    - Shot Actions
        
        ```jsx
        "use server"
        
        // import COLLECTION from '../model/collectionModel';
        import { getErrorMessage } from "@/utils/helper"
        
        import SHOT from "../model/shot.model"
        import VENDOR from "../model/vendor.model"
        import { connectToDB } from "../mongoose"
        
        //-------------NEW SHOT-------------//
        export const addShot = async (body: any) => {
          try {
            connectToDB()
            const shot = await new SHOT(body)
            await shot.save()
        
            const vendor = await VENDOR.findOne({ _id: body.owner })
        
            if (!shot || !vendor) {
              throw new Error("Unable to add shot!")
            }
        
            vendor.ownShot.push(shot._id)
            await vendor.save()
        
            return { shot }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------GET ALL SHOT-------------//
        export const getShot = async (type: string, limit = 10) => {
          try {
            connectToDB()
            let where = {}
            if (type) {
              //@ts-ignore - typescript is not recognizing the regex
              const types = type?.split(",").map((t) => new RegExp(t.trim(), "i"))
              where = { tags: { $in: types } }
            }
            const shots = await SHOT.find(where)
              .limit(limit)
              .select("title category description tags images owner")
              .populate("owner", "name follower following likedshot email")
              .sort("-createdAt")
            if (!shots) {
              throw Error("NO SHOT FOUND")
            }
            return { shots }
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        
        //-------------GET SHOT BY ID-------------//
        export const getShotById = async (id: string) => {
          try {
            connectToDB()
            const shot = await SHOT.findById(id)
              .select("title category description tags images owner")
              .populate("owner", "name profilePic email")
        
            if (!shot) {
              throw Error("NO SHOT FOUND")
            }
            return shot
          } catch (error) {
            return {
              error: getErrorMessage(error),
            }
          }
        }
        ```
        
3. Nextjs API routes
    - /vendor/route.ts
        
        ```jsx
        import { NextResponse } from "next/server"
        
        import { getVendorProfile, updateVendor } from "@/lib/actions/vendor.actions"
        import { IVendor } from "@/lib/model/types/vendor-type"
        
        export async function GET() {
          const { vendor, error } = await getVendorProfile()
          if (error) {
            return NextResponse.error()
          }
          return NextResponse.json(vendor)
        }
        
        export async function POST(request: Request) {
          const body = await request.json()
          const { vendor, error } = (await updateVendor(body)) as { vendor: IVendor; error?: undefined }
          if (error) {
            return NextResponse.error()
          }
          return NextResponse.json(vendor)
        }
        
        ```
        
    - /shot/route.ts
        
        ```jsx
        import { NextRequest, NextResponse } from "next/server"
        
        import { addShot } from "@/lib/actions/shot.actions"
        
        export async function POST(req: NextRequest) {
          try {
            const body = await req.json()
            if (!body || typeof body !== "object") {
              return NextResponse.error()
            }
        
            const shot = await addShot(body)
            if (!shot) {
              return NextResponse.error()
            }
        
            return NextResponse.json(shot)
          } catch (error) {
            console.error(error)
            return NextResponse.error()
          }
        }
        
        ```
        
    - /shot/[shotId]/route.ts
        
        ```jsx
        import { NextRequest, NextResponse } from "next/server"
        
        import { getShotById } from "@/lib/actions/shot.actions"
        
        export async function GET(request: NextRequest, { params }: { params: { shotId: string } }) {
          try {
            const shotId = params.shotId
            const shot = await getShotById(shotId)
            if (!shot) return NextResponse.json({ shot: {} })
        
            return NextResponse.json(shot)
          } catch (error) {
            return NextResponse.json({ error })
          }
        }
        
        ```
        
    - /uploadthing/route.ts
        
        ```jsx
        import { createNextRouteHandler } from "uploadthing/next"
        
        import { ourFileRouter } from "./core"
        
        // Export routes for Next App Router
        export const { GET, POST } = createNextRouteHandler({
          router: ourFileRouter,
        })
        
        // ALSO Make a separate core.ts file in the same folder
        
        import { createUploadthing, type FileRouter } from "uploadthing/next"
        
        const f = createUploadthing()
        
        export const ourFileRouter = {
          imageUploader: f({ image: { maxFileSize: "4MB" } }).onUploadComplete(async (data) => {
            return { data }
          }),
        } satisfies FileRouter
        
        export type OurFileRouter = typeof ourFileRouter
        ```
        
    

---

# Frontend(UI) Setup

Here, It has mainly 5-6 pages (routes).

```jsx
├── /home
│   ├── /designs
│   │   ├── /upload
│   │   └── /[shotId]
│   ├── /profile
│   │   └── /edit
```
