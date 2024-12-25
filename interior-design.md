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

### Home Page

- page.tsx
    
    ```jsx
    import type { NextPage } from "next"
    import Image from "next/image"
    import Link from "next/link"
    import { SHOTDATA } from "@/utils/dummy"
    
    import { getShot } from "@/lib/actions/shot.actions"
    import Hero from "@/components/hero"
    import IconList from "@/components/motion/icon-list"
    import RectangleCard from "@/components/rectangle-card"
    
    const Home: NextPage = async () => {
      const { shots } = await getShot("", 10)
    
      return (
        <section className="bg-dark">
          <Hero />
          <IconList />
    
          {/* SHOTS SECTION */}
          <h1 className="text-gray pt-16 text-center text-4xl font-bold">SHOTS</h1>
          <p className="text-gray mt-4 text-center">
            Upload Interior Design Shots Or Get Inspired <br /> By Other Designer&apos;s Works
          </p>
    
          <div className="padding py-16 text-2xl sm:text-4xl">
            {SHOTDATA.map((_, i) => (
              <Link
                href={`/designs?type=${_.type}`}
                className="border-gray  group flex items-center justify-between border-y p-4 hover:cursor-pointer hover:bg-[#292929]  "
                key={i}
              >
                <h3 className="text-gray group-hover:text-white sm:my-2 ">{_.title}</h3>
                <Image
                  src={"/arrow.png"}
                  alt="arrow icon"
                  height={50}
                  width={50}
                  priority
                  className="hidden bg-primary  p-2 md:group-hover:block "
                />
              </Link>
            ))}
          </div>
    
          {/* WEBSITE TEMPLATE IMAGES */}
    
          <Image src={"/imac.png"} alt="music bg" height={700} width={500} className="m-auto my-8 hidden md:block" />
          <Image src={"/phone.png"} alt="music bg" height={350} width={350} className="m-auto my-8 md:hidden" />
    
          {/* SHOT LISTS */}
          <h1 className="padding text-gray pt-16 text-center text-4xl font-bold uppercase">
            OVER <span className="text-primary">205+</span> Shots of <br /> INTERIOR dESIGN
          </h1>
    
          <div className="padding grid grid-cols-2 gap-4 md:grid-cols-5">
            {shots &&
              shots.map((_, i) => (
                <div key={i}>
                  <span className=" absolute m-2 rounded-2xl bg-pink-500 px-2 py-0 text-sm text-white">24k</span>
                  <Link href={`/designs/${_._id}`}>
                    <Image
                      src={_.images[0].url}
                      height={250}
                      width={250}
                      alt={"man"}
                      className="h-40 rounded object-fill hover:cursor-pointer "
                    />
                  </Link>
                  {/* <p>{_.title}</p> */}
                </div>
              ))}
          </div>
          <p className="border-gray m-auto mt-6 w-3/4 border-b pb-16 text-center">
            <Link href={"/designs"} className="rounded-md bg-primary px-6 py-2 text-white">
              Check More
            </Link>
          </p>
    
          <div className="my-16 grid grid-cols-5 justify-items-start">
            <div className="hidden lg:col-span-2 lg:block ">
              <Image src={"/saly.png"} height={500} width={500} alt={"saly"} />
            </div>
            <div className="padding col-span-5 mr-4 sm:mr-8 lg:col-span-3 xl:mr-32">
              <h1 className="text-gray pt-8 text-4xl font-bold   uppercase">
                WHAT OUR
                <span className="text-primary"> USERS</span>
                <br /> SAY ABOUT US
              </h1>
              <p className="text-gray my-6">
                Our users are our strength. We do every thing possible to make their experience unique and effortless.
              </p>
              <div className="card text-gray my-4 p-4 ">
                <p>
                  Interio Design is a true treasure trove for interior enthusiasts! The site boasts an exquisite array of design
                  inspirations, from sleek modernism to cozy rustic vibes. Navigating the user-friendly interface is a breeze, and the
                  curated galleries provide endless ideas. Whether you&apos;re a novice or a seasoned designer, this site is a must-visit
                  for endless creative sparks!
                </p>
    
                <h6 className="mt-4 font-semibold text-primary">STIEVE JOHN MATT</h6>
                <p>Interior Designer</p>
              </div>
            </div>
          </div>
    
          <RectangleCard />
          {/* FOOTER */}
          <div className="z-0 flex h-auto flex-col items-center justify-center gap-y-2 bg-primary py-24 text-center leading-10 text-[rgba(255,255,255,0.75)] ">
            <Link href="/">
              <Image src={"/interio.png"} alt="interio logo" height={40} width={40} />
            </Link>
            <p className="px-2 md:px-6 lg:px-10">
              Modern Designs | Minimal Designs | Luxurious Designs | Space Saving Designs <br /> Dark themed Designs | Hotel Room Designs |
              Terms & Conditions Privacy Policy | About us
            </p>
            <p>Handcrafted by Interio © {new Date().getFullYear()}</p>
            <p>Made with 💖 </p>
          </div>
        </section>
      )
    }
    
    export default Home
    
    ```
    
- layout.tsx
    
    ```jsx
    import { TailwindIndicator } from "../components/tailwind-indicator"
    
    import "./globals.css"
    
    import { Metadata } from "next"
    import { Roboto } from "next/font/google"
    import ReduxProvider, { QueryProvider } from "@/utils/providers"
    import { NextSSRPlugin } from "@uploadthing/react/next-ssr-plugin"
    import { extractRouterConfig } from "uploadthing/server"
    
    import { Toaster } from "@/components/ui/sonner"
    
    import { ourFileRouter } from "./api/uploadthing/core"
    import ExtraComponents from "./extra"
    
    const roboto = Roboto({
      subsets: ["latin"],
      weight: "500",
    })
    
    export const metadata: Metadata = {
      title: "Interior Design | Home",
      description: "Interior Design Shots, Get Inspired By Other Designer's Works",
    }
    
    export default function RootLayout({ children }: { children: React.ReactNode }) {
      return (
        <html lang="en">
          <body className={roboto.className}>
            <QueryProvider>
              <ReduxProvider>
                {children}
                <ExtraComponents />
              </ReduxProvider>
            </QueryProvider>
            <Toaster />
            <NextSSRPlugin routerConfig={extractRouterConfig(ourFileRouter)} />
            <TailwindIndicator />
          </body>
        </html>
      )
    }
    
    ```
    

### Design Pages

- /design/page.tsx
    
    ```jsx
    import { shotData } from "@/types"
    import { getShot } from "@/lib/actions/shot.actions"
    import ShotCard from "./shot-card"
    
    type Props = {
      searchParams: { type: string }
    }
    export default async function Designs({ searchParams }: Props) {
      const type = searchParams.type
      const shots: shotData[] = []
      try {
        const data = await getShot(type)
        if (data) {
          shots.push(...(data.shots as shotData[]))
        }
      } catch (error) {
        console.log(error)
      }
    
      return (
        <>
          {shots?.length > 0 ? (
            <div className="grid grid-cols-1 justify-items-center sm:grid-cols-2 md:grid-cols-3 md:gap-4 xl:grid-cols-4">
              {shots.map((shot: shotData) => (
                <ShotCard key={shot._id} shot={shot} />
              ))}
            </div>
          ) : (
            <p className="text-center">Nothing to show!</p>
          )}
        </>
      )
    }
    
    // SHOT CARD COMPONENT
    
    import React from "react"
    import Image from "next/image"
    import Link from "next/link"
    import { shotData } from "@/types"
    
    import { Icons } from "@/components/icons"
    
    export default function ShotCard({ shot }: { shot: shotData }) {
      return (
        <div key={shot._id} className="mb-2 sm:mb-4">
          <Link href={`/designs/${shot._id}`}>
            <Image
              src={shot.images[0].url}
              alt="l1img"
              height={350}
              quality={100}
              width={370}
              className="duration-400 h-52 w-80 cursor-pointer rounded object-fill transition-all hover:scale-95"
            />
          </Link>
          <div className="text-gray flex justify-between px-2 py-2">
            <span className="text-xs md:text-sm">
              <Icons.BsChatDots className="inline" /> 1.1k
            </span>
            <p className="flex gap-2">
              <span className="text-xs md:text-sm">
                <Icons.AiOutlineHeart className="inline" /> 1.1k
              </span>
              <span className="text-xs md:text-sm">
                <Icons.AiOutlineEye className="inline" /> 1.1k
              </span>
            </p>
          </div>
        </div>
      )
    }
    
    ```
    
- /design/[shotId]/page.tsx
    
    ```jsx
    import Image from "next/image"
    import { shotData } from "@/types"
    
    import { getShot, getShotById } from "@/lib/actions/shot.actions"
    import { Icons } from "@/components/icons"
    
    import ShotCard from "../shot-card"
    
    export type Props = {
      params: {
        shotId: string
      }
    }
    
    const ShotId = async ({ params }: Props) => {
      const shot = await getShotById(params.shotId)
      const moreShot = await getShot("", 4)
    
      if (!shot || shot.error) return <p className="text-center">Shot not found!</p>
    
      return (
        <>
          <Image
            src={shot?.images[0]?.url ? shot.images[0].url : "/group1/png"}
            className="h-[700px] w-[1200px] rounded"
            alt="bed"
            height={500}
            width={1400}
          />
          <div className="my-8 flex justify-between">
            <div className="flex gap-x-4">
              <Image src={shot.owner.profilePic || "/user.png"} height={40} width={40} alt="man dp" className="rounded-lg" />
              <div>
                <h1>{shot?.title}</h1>
                <p className="text-gray text-xs">{shot?.owner?.name}</p>
              </div>
            </div>
            <div className="flex gap-x-4 ">
              <button className="cborder bg-trans rounded px-4 py-2">Save</button>
              <button className="bg-trans rounded border border-pink-500 px-4 py-2">
                <Image src={"/pheart.png"} alt="heart-icon" height={20} width={20} className="inline" /> Like
              </button>
            </div>
          </div>
    
          <div className="my-8 flex items-center justify-center gap-4">
            <button className="cborder bg-trans rounded px-4 py-2">
              <Icons.BsChatDots />
            </button>
            <button className="cborder bg-trans rounded px-4 py-2">
              <Icons.FiFolderMinus />
            </button>
            <button className="cborder bg-trans rounded px-4 py-2">
              <Icons.BsShareFill />
            </button>
          </div>
          <p className="border-gray my-8 w-full border-[0.5px]" />
    
          <p>{shot?.description}</p>
    
          <h2>I am available for new projects</h2>
          <p className="my-4">
            📪 Email:
            <a href={`mailto:${shot.owner.email}`}>
              <span className="text-primary"> {shot?.owner?.email}</span>
            </a>
          </p>
          <p className="border-gray my-8 w-full border-[0.5px]" />
          {/* {shot?.owner?._id === vendor.v_id && (
                <div className='my-8 flex items-center justify-center gap-4'>
                  <button className='cborder rounded bg-trans px-4 py-2'>
                    Edit
                  </button>
                  <button className='cborder rounded bg-trans px-4 py-2'>
                    Edit Details
                  </button>
                  <button className='cborder rounded bg-trans px-4 py-2'>
                    Delete
                  </button>
                </div>
              )} */}
    
          <div className="flex flex-col gap-y-4">
            <div className="flex justify-between">
              <p>Similar Shots</p>
              <p className="cursor-pointer text-primary hover:underline">View Profile</p>
            </div>
            <div className="flex gap-4 overflow-x-auto ">
              {moreShot &&
                moreShot.shots &&
                moreShot.shots.length > 0 &&
                moreShot.shots.map((shot) => <ShotCard key={shot._id} shot={shot as shotData} />)}
            </div>
          </div>
        </>
      )
    }
    
    export default ShotId
    
    ```
    
- /design/upload/page.tsx
    
    ```jsx
    "use client"
    
    import React, { useState } from "react"
    import { useRouter } from "next/navigation"
    import { TAGS } from "@/utils/dummy"
    import { UploadDropzone } from "@/utils/uploadthing"
    import { joiResolver } from "@hookform/resolvers/joi"
    import axios from "axios"
    import Joi from "joi"
    import { SubmitHandler, useForm } from "react-hook-form"
    import { toast } from "sonner"
    
    import { Button } from "@/components/ui/button"
    import { Checkbox } from "@/components/ui/checkbox"
    import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form"
    import { Input } from "@/components/ui/input"
    import { Textarea } from "@/components/ui/textarea"
    
    interface IFormInput {
      description: string
      shotUrl: string
      tags: string[]
      title: string
    }
    
    const schema = Joi.object({
      description: Joi.string().min(200).max(1000).required().messages({
        "string.empty": `Description cannot be empty`,
        "any.required": `Please provide a description for your shot. It will be used as the description of the shot.`,
        "string.min": `Description should have a minimum length of {#limit}`,
        "string.max": `Description should have a maximum length of {#limit}`,
      }),
      shotUrl: Joi.string().required().messages({
        "string.empty": `Shot Url cannot be empty`,
        "any.required": `Please provide a url for your shot. It will be used as the image of the shot.`,
      }),
      title: Joi.string().required().messages({
        "string.empty": `Title cannot be empty`,
        "any.required": `Please provide a title for your shot. It will be used as the title of the shot.`,
      }),
      tags: Joi.array().items(Joi.string()).required().messages({
        "any.required": `Please provide a tags for your shot. It will be used as the tags of the shot.`,
      }),
    })
    
    const Upload = () => {
      const router = useRouter()
      const [loading, setLoading] = useState<boolean>(false)
      const form = useForm<IFormInput>({
        resolver: joiResolver(schema),
      })
    
      const onSubmit: SubmitHandler<IFormInput> = async (val) => {
        setLoading(true)
        toast.info("Uploading Shot...")
        try {
          const { data } = await axios.post("/api/shots", {
            role: "vendor",
            description: val.description,
            images: {
              title: "Hotel Room",
              url: val.shotUrl,
            },
            tags: val.tags,
            category: "Furniture",
            title: val.title,
            owner: localStorage.getItem("v_id"),
          })
          if (!data) {
            return
          }
          toast.success("Shot uploaded successfully")
          router.push(`/designs/${data.shot._id}`)
        } catch (error) {
          toast.error("Shot upload failed")
        } finally {
          setLoading(false)
        }
      }
    
      function addTags(tag: string, tags: string[]) {
        if (tags) {
          return [...tags, tag]
        } else {
          return [tag]
        }
      }
    
      return (
        <>
          <h1 className="mt-2 text-center">What have you been working on?</h1>
    
          <Form {...form}>
            <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
              <FormField
                control={form.control}
                name="title"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Title</FormLabel>
                    <FormControl>
                      <Input placeholder="enter title" {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="description"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Description</FormLabel>
                    <FormControl>
                      <Textarea rows={12} placeholder="Tell us a little bit about your shot" className="resize-none" {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <p className="text-muted-foreground">Upload Shot</p>
              <UploadDropzone
                endpoint="imageUploader"
                className="h-60 w-full rounded bg-background px-4 text-muted-foreground"
                onClientUploadComplete={(res) => {
                  form.setValue("shotUrl", res[0].url)
                  setLoading(false)
                }}
                onUploadBegin={() => {
                  setLoading(true)
                }}
                onUploadError={(error: Error) => {
                  toast.error(error.message)
                }}
                config={{
                  mode: "auto",
                }}
              />
              <FormField
                control={form.control}
                name="tags"
                render={() => (
                  <FormItem>
                    <FormLabel className="mb-2 text-base">Tags</FormLabel>
                    <div className="flex gap-2">
                      {TAGS.map((tag) => (
                        <FormField
                          key={tag.id}
                          control={form.control}
                          name="tags"
                          render={({ field }) => {
                            return (
                              <FormItem key={tag.id} className="flex items-start space-x-2 space-y-0">
                                <FormControl>
                                  <Checkbox
                                    checked={field.value?.includes(tag.id)}
                                    onCheckedChange={(checked) => {
                                      return checked
                                        ? field.onChange(addTags(tag.id, field.value))
                                        : field.onChange(field.value?.filter((value) => value !== tag.id))
                                    }}
                                  />
                                </FormControl>
                                <FormLabel className="font-normal">{tag.label}</FormLabel>
                              </FormItem>
                            )
                          }}
                        />
                      ))}
                    </div>
                    <FormMessage />
                  </FormItem>
                )}
              />
    
              <Button type="submit" className="w-full" disabled={loading}>
                Upload Shot
              </Button>
            </form>
          </Form>
        </>
      )
    }
    
    export default Upload
    
    ```
    
- /layout.tsx
    
    ```jsx
    "use client"
    
    import type { ReactElement } from "react"
    import Image from "next/image"
    import Link from "next/link"
    import { usePathname } from "next/navigation"
    import { useAppDispatch, useAppSelector } from "@/context/hook"
    import { isLogin, setLogout, togglePanel, vendor as vd } from "@/context/theme"
    import { EXCLUDE_PATHS, START_WITH_PATHS } from "@/utils/dummy"
    import clsx from "clsx"
    import { toast } from "sonner"
    
    import { vendorLogout } from "@/lib/actions/vendor.actions"
    import { Icons } from "@/components/icons"
    
    import DesignNav from "./design-nav"
    
    type Props = {
      children?: React.ReactNode
      way?: "without" | "with"
    }
    
    export default function LeftSideBar({ children, way }: Props): ReactElement {
      const dispatch = useAppDispatch()
      const vendor = useAppSelector(vd)
      const pathname = usePathname()
      const loginStatus = useAppSelector(isLogin)
    
      const logoutHandler = async () => {
        try {
          const data = await vendorLogout()
          if (data.error) {
            return toast.error(data.error)
          }
          toast.success("Logout Successfully!")
          dispatch(setLogout())
        } catch (error: any) {
          toast.error(error?.message)
          console.log("Some Error!", error)
        }
      }
      if (pathname.startsWith("/blogs/add")) return <>{children}</>
    
      return (
        <>
          <aside className="fixed hidden h-screen  md:flex">
            <div className="flex h-screen w-[60px] flex-col items-center justify-evenly border-r bg-black pb-[25vh] text-xl text-white lg:border-none ">
              <Link href="/">
                <Image src={"/interio.png"} alt="interio logo" height={35} width={35} />
              </Link>
              <Link href="/designs">
                <Icons.AiFillAppstore />
              </Link>
              <Link href="/profile">
                <Icons.AiOutlineUser />
              </Link>
              <button onClick={() => dispatch(togglePanel("invite"))}>
                <Icons.HiOutlineMail />
              </button>
              <button onClick={() => dispatch(togglePanel("collection"))}>
                <Icons.FiFolderMinus />
              </button>
              <p className="border-gray w-[2vw] border"> </p>
              <button title="Coming Soon!">
                <Icons.FiSettings />
              </button>
    
              <div className="cursor-pointer">{loginStatus && <Icons.HiOutlineLogout onClick={logoutHandler} />}</div>
    
              {loginStatus && (
                <Link href="/profile" className="absolute bottom-8 ">
                  <Image
                    src={vendor?.profilePic || "/user.png"}
                    alt="interio logo"
                    height={40}
                    width={40}
                    className="h-10 w-10 rounded-full object-fill "
                  />
                </Link>
              )}
            </div>
            <div className="bluebg hidden h-screen w-[220px] flex-col items-center justify-evenly pb-[35vh] text-xl text-secondary lg:flex  ">
              <div className="mt-3 w-[200px]">
                {/* ?.split(' ')[0] */}
                <h3 className="font-sm font-medium">Hello {vendor?.name?.split(" ")[0]},</h3>
                <p className="text-light text-xs">Check out your store analysis</p>
              </div>
    
              <Link
                href="/designs"
                className={clsx(
                  { "bg-secondary/70 text-white": pathname == "/designs" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.HiOutlinePhotograph size={24} />
                <div>
                  <h3 className="font-sm font-medium">10k+</h3>
                  <p className="text-light text-xs">Inspirations for you</p>
                </div>
              </Link>
              <div
                className={clsx(
                  { "bg-secondary text-white": pathname == "/suitcase" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.RiSuitcaseLine size={24} />
                <div>
                  <h3 className="font-sm font-medium">123+</h3>
                  <p className="text-light text-xs">
                    Find Work <br /> (coming soon)
                  </p>
                </div>
              </div>
              <div
                className={clsx(
                  { "bg-secondary text-white": pathname == "/user" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.AiOutlineUser size={24} />
                <div>
                  <h3 className="font-sm font-medium">104+</h3>
                  <p className="text-light text-xs">
                    Hire Designer <br /> (coming soon)
                  </p>
                </div>
              </div>
              <Link
                href={`/profile/chat?v_id=${vendor._id}`}
                className={clsx(
                  {
                    "bg-secondary text-white": pathname == "/profile/chat" || pathname == "/profile/chat/[ChatId]",
                    "pointer-events-none": !loginStatus,
                  },
                  "pointer-events-none flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.BsChatDots size={24} />
                <div>
                  {loginStatus ? (
                    <>
                      <h3 className="font-sm font-medium">{"5+"}</h3>
                      <p className="text-light text-xs">Project messages</p>
                    </>
                  ) : (
                    <p className="text-light text-sm">Login to chat</p>
                  )}
                </div>
              </Link>
            </div>
          </aside>
          <div
            className={clsx(
              {
                "px-4 pt-4 text-white sm:px-8 md:pt-8 xl:px-10 ": way === "without",
              },
              "px-4 py-8 text-white sm:px-8 md:ml-[65px] md:pt-8 lg:ml-[280px] lg:w-auto xl:px-10"
            )}
          >
            {!EXCLUDE_PATHS.includes(pathname) && !START_WITH_PATHS.some((_) => pathname.startsWith(_)) && <DesignNav />}
            {children}
          </div>
        </>
      )
    }
    
    ```
    

### Profile Pages

- profile/page.tsx
    
    ```jsx
    import { formatDateInDMY } from "@/utils/helper"
    
    import { getVendorProfile } from "@/lib/actions/vendor.actions"
    import { Icons } from "@/components/icons"
    
    const User = async () => {
      const { vendor, error } = await getVendorProfile()
    
      if (error || !vendor)
        return (
          <div className="text-center">
            <h1 className="text-red-500">Some error at our end!</h1>
            <p className="text-gray">{error}</p>
          </div>
        )
    
      return (
        <section className="space-y-6">
          <div>
            <h1 className="mb-1 text-xl text-primary underline decoration-foreground underline-offset-2">Biography</h1>
            <p className="text-sm text-foreground">{vendor?.biography}</p>
          </div>
          <div>
            <h1 className="mb-1 text-xl text-primary underline decoration-foreground underline-offset-2">Skills</h1>
            <div className="text-gray mt-2 flex flex-wrap gap-2">
              {vendor?.skills.map((_, i) => (
                <span className="rounded-3xl bg-secondary px-4 py-1 capitalize text-white" key={i}>
                  {_}
                </span>
              ))}
            </div>
          </div>
    
          <div>
            <h1 className="mb-1 text-xl text-primary underline decoration-foreground underline-offset-2">Work History</h1>
            {vendor.workHistory.length > 0 ? (
              vendor?.workHistory.map((_, i) => (
                <div className="mb-4 grid grid-cols-2 items-end justify-between gap-2 text-sm text-foreground lg:grid-cols-4" key={i}>
                  <div>
                    <h2 className="mb-1 text-base">{_.title}</h2>
                    <h3 className="flex items-center gap-2 lg:ml-4">
                      <Icons.RiSuitcaseLine size={24} className="text-white" /> {_.company}
                    </h3>
                  </div>
                  <p className="flex items-center gap-1">
                    <Icons.Location size={24} className="text-white" /> {_.location}
                  </p>
                  <p className="flex items-center gap-1">
                    <Icons.Calender size={24} className="text-white" /> {formatDateInDMY(_.from)}
                  </p>
                  <p className="flex items-center gap-1">
                    <Icons.Calender size={24} className="text-white" />
                    {formatDateInDMY(_.to) || "Now"}
                  </p>
                </div>
              ))
            ) : (
              <p className="text-foreground">No work history found</p>
            )}
    
            <div>
              <h1 className="mb-1 text-xl text-primary underline decoration-foreground underline-offset-2">Looking For</h1>
              {vendor.lookingFor.length > 0 ? (
                <div className="text-foreground">
                  {vendor?.lookingFor.map((_, i) => (
                    <div key={i} className="mb-2">
                      <h2 className="mb-1 text-base">{_.title}</h2>
                      <h3 className="flex items-center gap-2 lg:ml-4">
                        <Icons.Location size={24} className="text-white" /> {_.location}
                      </h3>
                    </div>
                  ))}
                </div>
              ) : (
                <p className="text-foreground">No looking for found</p>
              )}
            </div>
          </div>
        </section>
      )
    }
    
    export default User
    
    ```
    
- profile/edit/page.ts
    
    ```jsx
    "use client"
    
    import { useEffect, useState } from "react"
    import Image from "next/image"
    import Link from "next/link"
    import { useRouter } from "next/navigation"
    import { TAGS } from "@/utils/dummy"
    import { joiResolver } from "@hookform/resolvers/joi"
    import axios from "axios"
    import Joi from "joi"
    import { SubmitHandler, useForm } from "react-hook-form"
    import { toast } from "sonner"
    
    import { Button, buttonVariants } from "@/components/ui/button"
    import { Checkbox } from "@/components/ui/checkbox"
    import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form"
    import { Input } from "@/components/ui/input"
    import { Textarea } from "@/components/ui/textarea"
    import { Icons } from "@/components/icons"
    
    interface IFormInput {
      name: string
      profilePic: string
      email: string
      contact: number
      address: string
      biography: string
      workHistory: object
      lookingFor: object
      skills: string[]
    }
    
    const schema = Joi.object({
      name: Joi.string().max(50).required().messages({
        "string.max": "Name must be at most {#limit} characters long",
        "any.required": "Name is required",
      }),
      profilePic: Joi.string(),
      contact: Joi.string().min(8).max(12).messages({
        "string.min": "Contact number should be atleast 8 digitd",
        "string.max": "Name must be at most 12 digits long",
      }),
      email: Joi.string()
        .email({ tlds: { allow: false } })
        .required()
        .messages({
          "string.empty": "Email is required",
          "any.required": "Email is required",
        }),
      address: Joi.string(),
      biography: Joi.string().max(500).messages({
        "string.max": "Bio must be at most {#limit} characters long",
      }),
      skills: Joi.array().items(Joi.string()).min(1).required().messages({
        "any.required": "Select atleast 1 skill.",
        "array.min": "Select atleast 1 skill.",
      }),
      // workHistory: Joi.array().items({
      //   title: Joi.string(),
      //   company: Joi.string(),
      //   location: Joi.string(),
      //   from: Joi.string(),
      //   to: Joi.string(),
      // }),
      // lookingFor: Joi.array().items({
      //   title: Joi.string(),
      //   location: Joi.string(),
      // }),
    })
    
    const Edit = () => {
      const [vendor, setVendor] = useState<IFormInput>()
      const [loading, setLoading] = useState<boolean>(false)
      const router = useRouter()
      const form = useForm<IFormInput>({
        resolver: joiResolver(schema),
        defaultValues: vendor,
      })
    
      const getVendor = async () => {
        try {
          const data = await axios.get(`/api/vendor`)
          if (data.status !== 200) throw new Error("Something went wrong")
          const vendor = data.data
          setVendor(vendor)
          if (vendor) {
            form.setValue("name", vendor.name)
            form.setValue("address", vendor.address)
            form.setValue("email", vendor.email)
            form.setValue("skills", vendor.skills)
            // form.setValue("workHistory", vendor.workHistory)
            // form.setValue("lookingFor", vendor.lookingFor)
            // vendor.contact && form.setValue("contact", vendor.contact)
            // vendor.biography && form.setValue("biography", vendor.biography)
            // vendor.profilePic && form.setValue("profilePic", vendor.profilePic)
          }
        } catch (error) {
          console.log(error)
        }
      }
      useEffect(() => {
        getVendor()
        // eslint-disable-next-line react-hooks/exhaustive-deps
      }, [])
    
      const onSubmit: SubmitHandler<IFormInput> = async (val) => {
        setLoading(true)
        try {
          // val.skills.filter((value) => value !== "")
          // remove repeated skills
    
          val.skills = val.skills.filter((skill, index) => {
            return val.skills.indexOf(skill) === index
          })
    
          const data = await axios.post(`/api/vendor`, val)
          console.log(data)
          toast.success("Profile updated successful")
          router.push("/profile")
        } catch (error) {
          toast.error("Update failed")
        } finally {
          setLoading(false)
        }
      }
    
      return (
        <>
          <Form {...form}>
            <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
              <div className="relative -mt-16 flex  items-center justify-between rounded bg-secondary/80 p-2 px-4 text-white md:-mt-24">
                <div className="flex items-center gap-4">
                  <Image
                    src={form.getValues("profilePic") || "/user.png"}
                    alt="user"
                    width={80}
                    height={80}
                    className="h-12 w-12 rounded-full md:h-20 md:w-20"
                  />
                  <Icons.Edit size={24} />
                </div>
                <Link
                  className={buttonVariants({
                    variant: "outline",
                  })}
                  href={"/profile"}
                >
                  Cancel Edit
                </Link>
              </div>
    
              <div className="grid gap-4 md:grid-cols-3">
                <FormField
                  control={form.control}
                  name="name"
                  render={({ field }) => (
                    <FormItem>
                      <FormLabel>Name</FormLabel>
                      <FormControl>
                        <Input type="name" {...field} defaultValue={vendor?.name} />
                      </FormControl>
                      <FormMessage />
                    </FormItem>
                  )}
                />
                <FormField
                  control={form.control}
                  name="email"
                  render={({ field }) => (
                    <FormItem>
                      <FormLabel>Email</FormLabel>
                      <FormControl>
                        <Input type="email" {...field} defaultValue={vendor?.email} />
                      </FormControl>
                      <FormMessage />
                    </FormItem>
                  )}
                />
                <FormField
                  control={form.control}
                  name="contact"
                  render={({ field }) => (
                    <FormItem>
                      <FormLabel>Contact</FormLabel>
                      <FormControl>
                        <Input type="number" {...field} defaultValue={vendor?.contact} />
                      </FormControl>
                      <FormMessage />
                    </FormItem>
                  )}
                />
              </div>
              <FormField
                control={form.control}
                name="address"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Address</FormLabel>
                    <FormControl>
                      <Textarea {...field} defaultValue={vendor?.address} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="biography"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Biography</FormLabel>
                    <FormControl>
                      <Textarea {...field} defaultValue={vendor?.biography} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="skills"
                render={() => (
                  <FormItem>
                    <FormLabel className="mb-2 text-base">Skills</FormLabel>
                    <div className="flex gap-2">
                      {TAGS.map((tag) => (
                        <FormField
                          key={tag.id}
                          control={form.control}
                          name="skills"
                          render={({ field }) => {
                            return (
                              <FormItem key={tag.id} className="flex items-start space-x-2 space-y-0">
                                <FormControl>
                                  <Checkbox
                                    checked={field.value?.includes(tag.id)}
                                    onCheckedChange={(checked) => {
                                      return checked
                                        ? field.onChange(addSkill(tag.id, field.value))
                                        : field.onChange(field.value?.filter((value) => value !== tag.id))
                                    }}
                                  />
                                </FormControl>
                                <FormLabel className="font-normal">{tag.label}</FormLabel>
                              </FormItem>
                            )
                          }}
                        />
                      ))}
                    </div>
                    <FormMessage />
                  </FormItem>
                )}
              />
    
              <Button type="submit" className="w-full" disabled={loading}>
                Save
              </Button>
            </form>
          </Form>
        </>
      )
    }
    
    export default Edit
    
    function addSkill(tag: string, tags: string[]) {
      if (tags) {
        return [...tags, tag]
      } else {
        return [tag]
      }
    }
    
    ```
    
- profile/[tab]/page.tsx
    
    ```jsx
    import { shotData } from "@/types"
    
    import { getTabs } from "@/lib/actions/vendor.actions"
    import ShotCard from "@/app/(designs)/designs/shot-card"
    
    const Profile = async ({ params }: { params: { tab: string } }) => {
      const { shots, error } = (await getTabs(params.tab)) as { shots: shotData[]; error?: string }
    
      if (!shots || shots.length == 0 || error) return <div className="text-center text-foreground">Nothing to show!</div>
    
      return (
        <>
          <div className="grid grid-cols-2 justify-items-center gap-4 md:grid-cols-3 xl:grid-cols-4">
            {/* @ts-ignore - d */}
            {shots && shots.map((shot: shotData) => <ShotCard key={shot._id} shot={shot} />)}
          </div>
        </>
      )
    }
    
    export default Profile
    
    ```
    
- /layout.tsx
    
    ```jsx
    "use client"
    
    import type { ReactElement } from "react"
    import Image from "next/image"
    import Link from "next/link"
    import { usePathname } from "next/navigation"
    import { useAppDispatch } from "@/context/hook"
    import { isLogin, togglePanel, vendor as vd } from "@/context/theme"
    import { useSelector } from "react-redux"
    import { toast } from "sonner"
    
    import { Button, buttonVariants } from "@/components/ui/button"
    import LeftSideBar from "@/app/(designs)/layout"
    
    export default function Layout({ children }: { children: React.ReactNode }): ReactElement {
      const pathname = usePathname()
      const loginStatus = useSelector(isLogin)
      const vendor = useSelector(vd)
      const dispatch = useAppDispatch()
    
      if (!loginStatus) {
        toast.error("Signup/Signin for checking profile!")
      }
      return (
        <LeftSideBar>
          <>
            <section className="flex-col">
              <Image key={2} src={"/coverimg.png"} alt="cover_image" className="block w-full rounded" height={170} width={740} />
              {loginStatus && pathname !== "/profile/edit" && (
                <div className="relative -mt-16 flex  items-center justify-between rounded bg-secondary/80 p-2 px-4 text-white md:-mt-24">
                  <div className="flex items-center gap-4">
                    <Image
                      src={vendor?.profilePic || "/user.png"}
                      alt="user"
                      width={80}
                      height={80}
                      className="h-12 w-12 rounded-full md:h-20 md:w-20"
                    />
                    <div>
                      <h1>{vendor?.name}</h1>
                      <p className="text-gray text-xs">0 Followers | 0 Following</p>
                    </div>
                  </div>
                  <Link
                    className={buttonVariants({
                      variant: pathname == "/profile/edit" ? "outline" : "default",
                    })}
                    href={pathname == "/profile/edit" ? "/profile" : "/profile/edit"}
                  >
                    {pathname == "/profile/edit" ? "Cancel Edit" : "Edit Profile"}
                  </Link>
                </div>
              )}
            </section>
            {loginStatus ? (
              <>
                {!(pathname == "/profile/edit") && (
                  <main>
                    <header className="text-gray my-2 grid grid-cols-4 items-center justify-between gap-2 max-md:flex max-md:overflow-x-scroll">
                      <Link
                        href={"/profile"}
                        className={`${buttonVariants({
                          variant: "secondary",
                        })} ${pathname == "/profile" ? "bg-secondary/50 text-primary" : "text-white"} min-w-fit`}
                      >
                        About me
                      </Link>
                      <Link
                        href={"/profile/work"}
                        className={`${buttonVariants({
                          variant: "secondary",
                        })} ${pathname == "/profile/work" ? "bg-secondary/80 text-primary" : "text-white"} min-w-fit`}
                      >
                        Work
                      </Link>
    
                      <Link
                        href={"/profile/liked-shot"}
                        className={`${buttonVariants({
                          variant: "secondary",
                        })} ${pathname == "/profile/liked-shot" ? "bg-secondary/80 text-primary" : "text-white"} min-w-fit`}
                      >
                        Liked Shots
                      </Link>
                      <Link
                        href={"/profile/collection"}
                        className={`${buttonVariants({
                          variant: "secondary",
                        })} ${pathname == "/profile/collection" ? "bg-secondary/80 text-primary" : "min-w-fit text-white"}
                        `}
                      >
                        Collections
                      </Link>
                    </header>
                    <p className="border-gray mb-4 w-full border-[0.5px] px-4" />
                  </main>
                )}
                {children}
              </>
            ) : (
              <div className="flex flex-col items-center justify-center">
                <h1 className="text-gray mt-8 text-center">
                  Login/Signup to access messages! or Back to{" "}
                  <Link href="/designs" className="text-primary underline">
                    Designs
                  </Link>{" "}
                </h1>
                <div className="mt-4 flex gap-4">
                  <Button className="mr-4 rounded bg-primary px-4 py-2" onClick={() => dispatch(togglePanel("signup"))}>
                    Sign up
                  </Button>
                  <Button className="rounded bg-secondary px-4 py-2" onClick={() => dispatch(togglePanel("signin"))}>
                    Sign in
                  </Button>
                </div>
              </div>
            )}
          </>
        </LeftSideBar>
      )
    }
    
    ```
    
- LeftSideBar - component
    
    ```jsx
    "use client"
    
    import type { ReactElement } from "react"
    import Image from "next/image"
    import Link from "next/link"
    import { usePathname } from "next/navigation"
    import { useAppDispatch, useAppSelector } from "@/context/hook"
    import { isLogin, setLogout, togglePanel, vendor as vd } from "@/context/theme"
    import { EXCLUDE_PATHS, START_WITH_PATHS } from "@/utils/dummy"
    import clsx from "clsx"
    import { toast } from "sonner"
    
    import { vendorLogout } from "@/lib/actions/vendor.actions"
    import { Icons } from "@/components/icons"
    
    import DesignNav from "./design-nav"
    
    type Props = {
      children?: React.ReactNode
      way?: "without" | "with"
    }
    
    export default function LeftSideBar({ children, way }: Props): ReactElement {
      const dispatch = useAppDispatch()
      const vendor = useAppSelector(vd)
      const pathname = usePathname()
      const loginStatus = useAppSelector(isLogin)
    
      const logoutHandler = async () => {
        try {
          const data = await vendorLogout()
          if (data.error) {
            return toast.error(data.error)
          }
          toast.success("Logout Successfully!")
          dispatch(setLogout())
        } catch (error: any) {
          toast.error(error?.message)
          console.log("Some Error!", error)
        }
      }
      if (pathname.startsWith("/blogs/add")) return <>{children}</>
    
      return (
        <>
          <aside className="fixed hidden h-screen  md:flex">
            <div className="flex h-screen w-[60px] flex-col items-center justify-evenly border-r bg-black pb-[25vh] text-xl text-white lg:border-none ">
              <Link href="/">
                <Image src={"/interio.png"} alt="interio logo" height={35} width={35} />
              </Link>
              <Link href="/designs">
                <Icons.AiFillAppstore />
              </Link>
              <Link href="/profile">
                <Icons.AiOutlineUser />
              </Link>
              <button onClick={() => dispatch(togglePanel("invite"))}>
                <Icons.HiOutlineMail />
              </button>
              <button onClick={() => dispatch(togglePanel("collection"))}>
                <Icons.FiFolderMinus />
              </button>
              <p className="border-gray w-[2vw] border"> </p>
              <button title="Coming Soon!">
                <Icons.FiSettings />
              </button>
    
              <div className="cursor-pointer">{loginStatus && <Icons.HiOutlineLogout onClick={logoutHandler} />}</div>
    
              {loginStatus && (
                <Link href="/profile" className="absolute bottom-8 ">
                  <Image
                    src={vendor?.profilePic || "/user.png"}
                    alt="interio logo"
                    height={40}
                    width={40}
                    className="h-10 w-10 rounded-full object-fill "
                  />
                </Link>
              )}
            </div>
            <div className="bluebg hidden h-screen w-[220px] flex-col items-center justify-evenly pb-[35vh] text-xl text-secondary lg:flex  ">
              <div className="mt-3 w-[200px]">
                {/* ?.split(' ')[0] */}
                <h3 className="font-sm font-medium">Hello {vendor?.name?.split(" ")[0]},</h3>
                <p className="text-light text-xs">Check out your store analysis</p>
              </div>
    
              <Link
                href="/designs"
                className={clsx(
                  { "bg-secondary/70 text-white": pathname == "/designs" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.HiOutlinePhotograph size={24} />
                <div>
                  <h3 className="font-sm font-medium">10k+</h3>
                  <p className="text-light text-xs">Inspirations for you</p>
                </div>
              </Link>
              <div
                className={clsx(
                  { "bg-secondary text-white": pathname == "/suitcase" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.RiSuitcaseLine size={24} />
                <div>
                  <h3 className="font-sm font-medium">123+</h3>
                  <p className="text-light text-xs">
                    Find Work <br /> (coming soon)
                  </p>
                </div>
              </div>
              <div
                className={clsx(
                  { "bg-secondary text-white": pathname == "/user" },
                  "flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.AiOutlineUser size={24} />
                <div>
                  <h3 className="font-sm font-medium">104+</h3>
                  <p className="text-light text-xs">
                    Hire Designer <br /> (coming soon)
                  </p>
                </div>
              </div>
              <Link
                href={`/profile/chat?v_id=${vendor._id}`}
                className={clsx(
                  {
                    "bg-secondary text-white": pathname == "/profile/chat" || pathname == "/profile/chat/[ChatId]",
                    "pointer-events-none": !loginStatus,
                  },
                  "pointer-events-none flex w-[200px] items-center gap-x-2 rounded-lg px-4 py-2"
                )}
              >
                <Icons.BsChatDots size={24} />
                <div>
                  {loginStatus ? (
                    <>
                      <h3 className="font-sm font-medium">{"5+"}</h3>
                      <p className="text-light text-xs">Project messages</p>
                    </>
                  ) : (
                    <p className="text-light text-sm">Login to chat</p>
                  )}
                </div>
              </Link>
            </div>
          </aside>
          <div
            className={clsx(
              {
                "px-4 pt-4 text-white sm:px-8 md:pt-8 xl:px-10 ": way === "without",
              },
              "px-4 py-8 text-white sm:px-8 md:ml-[65px] md:pt-8 lg:ml-[280px] lg:w-auto xl:px-10"
            )}
          >
            {!EXCLUDE_PATHS.includes(pathname) && !START_WITH_PATHS.some((_) => pathname.startsWith(_)) && <DesignNav />}
            {children}
          </div>
        </>
      )
    }
    
    ```
    

### Context Setup

- hook.ts
    
    ```jsx
    import { useDispatch, useSelector } from "react-redux"
    import type { TypedUseSelectorHook } from "react-redux"
    
    import type { AppDispatch, RootState } from "./store"
    
    // Use throughout your app instead of plain `useDispatch` and `useSelector`
    export const useAppDispatch: () => AppDispatch = useDispatch
    export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
    
    ```
    
- store.ts
    
    ```jsx
    import { configureStore } from "@reduxjs/toolkit"
    
    import themeReducer from "./theme"
    
    export const store = configureStore({
      reducer: {
        theme: themeReducer,
      },
    })
    
    // Infer the `RootState` and `AppDispatch` types from the store itself
    export type RootState = ReturnType<typeof store.getState>
    // Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
    export type AppDispatch = typeof store.dispatch
    
    ```
    
- theme.ts
    
    ```jsx
    import { createSlice } from "@reduxjs/toolkit"
    
    import type { RootState } from "./store"
    
    export interface themeState {
      sidebar: boolean
      isLogin: boolean
      showPanel: boolean
      panelFor: string
      vendor: {
        name?: string
        _id?: string
        token?: string
        profilePic?: string
      }
    }
    
    const initialState: themeState = {
      sidebar: false,
      isLogin: false,
      showPanel: false,
      panelFor: "",
      vendor: {
        name: "",
        _id: "",
        token: "",
        profilePic: "",
      },
    }
    
    export const themeSlice = createSlice({
      name: "theme",
      initialState,
      reducers: {
        togglePanel: (state, actions) => {
          const panelType = actions.payload
    
          if (panelType === "HIDE") {
            state.showPanel = false
            state.panelFor = ""
          } else {
            state.showPanel = true
            state.panelFor = panelType
          }
        },
    
        showSidebar: (state) => {
          state.sidebar = true
        },
        hideSidebar: (state) => {
          state.sidebar = false
        },
        setLogin: (state, actions) => {
          state.isLogin = true
          const { name, _id, token, profilePic } = actions?.payload
          state.vendor.name = name
          state.vendor._id = _id
          state.vendor.token = token
          state.vendor.profilePic = profilePic
    
          if (name && _id) {
            localStorage.setItem("vendor", name)
            localStorage.setItem("v_id", _id)
          }
        },
        setLogout: (state) => {
          state.isLogin = false
          localStorage.removeItem("vendor")
          localStorage.removeItem("v_id")
        },
      },
    })
    
    // Action creators are generated for each case reducer function
    export const { showSidebar, hideSidebar, setLogin, setLogout, togglePanel } = themeSlice.actions
    export const sidebar = (state: RootState) => state.theme.sidebar
    export const isLogin = (state: RootState) => state.theme.isLogin
    export const panelFor = (state: RootState) => state.theme.panelFor
    export const showPanel = (state: RootState) => state.theme.showPanel
    export const vendor = (state: RootState) => state.theme.vendor
    export default themeSlice.reducer
    
    ```
    

## Conclusion

This project documentation provides a comprehensive overview of the Interio Full Stack application, detailing its various components and functionalities. From user profiles to vendor interactions, the documentation covers all essential aspects to ensure a smooth and efficient development process.

By following the guidelines and code examples provided, you can effectively contribute to and enhance the application's features. Continued collaboration and adherence to best practices will be key to the project's success.

---

## **Useful Resources**

1. https://nextjs.org/
2. https://ui.shadcn.com/
3. https://tailwindcss.com/
4. https://react-hook-form.com/
5. https://www.mongodb.com/
6. https://mongoosejs.com/
7. https://joi.dev/
8. https://uploadthing.com/
