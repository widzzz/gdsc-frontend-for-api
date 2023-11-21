<script>
  import "../app.pcss";
  import {
    Navbar,
    NavBrand,
    NavLi,
    NavUl,
    NavHamburger,
    ImagePlaceholder,
    Skeleton,
    TextPlaceholder,
    Card,
    Gallery,
  } from "flowbite-svelte";
  import { onMount } from "svelte";
  import {
    Footer,
    FooterBrand,
    FooterCopyright,
    FooterIcon,
    FooterLink,
    FooterLinkGroup,
  } from "flowbite-svelte";
  import { GithubSolid } from "flowbite-svelte-icons";
</script>

<svelte:head>
  <title>GDSC UNNES</title>
  <link
    rel="icon"
    type="image/png"
    href="https://res.cloudinary.com/startup-grind/image/upload/c_fill,dpr_2.0,f_auto,g_center,h_32,q_auto:good,w_32/v1/gcs/platform-data-dsc/contentbuilder/favicon.png"
  />
</svelte:head>

<Navbar class="px-2 sm:px-4 py-2.5 fixed w-full z-20 top-0 left-0 border-b">
  <NavBrand href="/">
    <img
      src="https://res.cloudinary.com/startup-grind/image/upload/c_fill,dpr_2,f_auto,g_face,h_200,q_auto:good,w_200/v1/gcs/platform-data-dsc/events/GDSC-Logo_rgMpMHD.png"
      class="mr-3 h-6 sm:h-9"
      alt="Flowbite Logo"
    />
    <span
      class="self-center whitespace-nowrap text-xl font-semibold dark:text-white"
      >GDSC UNNES</span
    >
  </NavBrand>
  <NavHamburger />
  <NavUl>
    <!-- <NavLi href="/" active={true}>Home</NavLi>
    <NavLi href="/about">About</NavLi>
    <NavLi href="/docs/components/navbar">Navbar</NavLi>
    <NavLi href="/pricing">Pricing</NavLi>
    <NavLi href="/contact">Contact</NavLi> -->
    <NavLi href="https://github.com/widzzz"><GithubSolid /></NavLi>
  </NavUl>
</Navbar>
<slot />
<Footer class="mt-24">
  <FooterCopyright
    by="@widzzz"
    year={2023}
    copyrightMessage={"Powered by SvelteKit"}
  />
</Footer>
