<script>
  import {get} from 'svelte/store';
  import {page, session} from '$app/stores'
  import ThemeSelect from '$lib/featureComponent/theme-select.svelte'
  // 브라우져 세션 값 있으면 갖고옴
  const sessionId = get(session).user?get(session).user.sknowSession:""
  // 레이아웃페이지에서 가져온 로그인 아이디
  export let loginId = "unknown"
  // const loginId = $page.stuff.loginId || ""
  const nav = [
      { title: `${loginId}'sKnow`, link: `/${loginId}` },
      { title: 'School', link: '/school' },
      { title: 'Agora', link: '/agora' },
      { title: 'Team', link: '/team' },
    ]
  // 현재 주소
  $: link=$page.url.pathname

  const logout = async () => {
    // console.log("1 로그아웃 버튼 클릭");
    // console.log("2 sessionId: ", sessionId);
    const res = await fetch('/api/acount/removeSession', {
      method: 'POST',
      body: JSON.stringify({
        sessionId
      })
    }).then(res => res.json())
    .then(res => {
      // console.log("8", res.message)
      window.location.href = `/acount/login`
    })
    .catch(err=>console.log(err))
  }
</script>

<div class="navbar">
  <div class="navbar-start">
  <!-- --네비 좌 로고------------------------------------------ -->
    <a class="text-2xl font-bold p-3" href="/">s<span class="text-orange-400">K</span>now</a>
  </div>
  <div class="navbar-center hidden lg:flex">
  <!-- --가운데------------------------------------------ -->
    <ul class="flex gap-7 p-0">
			{#each nav as item}
				<li class="hover:text-[#ff9abc] transition-all duration-500">
          <!-- 현재 링크가 아이템링크와 같으면 active 클래스 추가 -->
					<a class:active={link === item.link} class="text-xl" href={item.link}>
            {item.title}
          </a>
				</li>
			{/each}
    </ul>
  </div>
  <div class="navbar-end pr-3">
  <!-- --마지막------------------------------------------ -->
    {#if loginId}
      <div class="dropdown dropdown-end">
        <div tabindex="0" class="btn btn-circle btn-ghost m-1">{loginId}</div>
        <ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52">
          <li><a>🍔 Profile</a></li>
          <li><a>🥞 Edit profile</a></li>
          <li><div on:click={logout}>🥫 Log out</div></li>
          <li><a>🍖 Etc...</a></li>
        </ul>
      </div>
      {:else}
      <div class="dropdown dropdown-end">
        <div tabindex="0" class="btn btn-circle btn-ghost m-1">Acount</div>
        <ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52">
          <li><a href="/acount/login">🍔 Login</a></li>
          <li><a href="/acount/createAcount">🥞 Create Acount</a></li>
        </ul>
      </div>
    {/if}
    <ThemeSelect />
  </div>
</div>

<style>
  .active {
    text-decoration: underline;
		font-size: 1.25rem;
		font-weight: bold;
    color: #ff9abc;
		/* transition: all 0.5s;  */
  }
</style>