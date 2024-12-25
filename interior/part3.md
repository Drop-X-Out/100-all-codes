# Interio - Full Stack

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
