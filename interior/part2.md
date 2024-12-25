# Interio - Full Stack

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
    
