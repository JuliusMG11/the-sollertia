<template>
    <header class="header">
       <div class="container mx-auto">
        <div class="header-section flex justify-between items-center">
            <div class="left-side">
                 <SharedLogoLink class="logo" color="white" />
            </div>
            <div class="right-side flex items-center">
                <div class="magic-button">
                    <a href="#">
                        magiceden mint
                    </a>
                </div>
                <div class="open-icon">
                    <img src="@/assets/img/svg/menu-icon.svg" alt="">
                </div>
            </div>
            
        </div>
       </div>
       <div class="popup-header">
            <nav class="menu">
                <NuxtLink class="menu-link" to="/" title="Úvod">Úvod</NuxtLink>
            </nav>
       </div>
    </header>
</template>

<style lang="scss" scoped>
.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 50;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px;
    background: rgba(18, 18, 18, 0.20);
backdrop-filter: blur(4px);
border-bottom: 1px solid #ffffff4e;
}

.right-side {
    gap: 22px;

    .magic-button {
        a {
            padding: 8px 32px;
            border: solid 1px #ffffff;
            background: #ffffff;
            color: #121212;
            font-weight: 700;
            text-transform: uppercase;
        }
    }

    .open-icon {
        width:42px;
        height: 42px;
        
        img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }
}

.popup-header {
    display: none;
}

.menu-link {
    text-decoration: none;
    display: block;
    color: $color-text-dark-theme;
    padding: 10px 20px;
    transition: all 0.2s ease;
    position: relative;

    &:hover {
        opacity: 0.8;
    }

    /* &.router-link-exact-active::before {
        content: '';
        position: absolute;
        bottom: 0;
        width: 20px;
        height: 2px;
        left: 50%;
        margin-left: -10px;
        background-color: $color-text-dark-theme;
    } */
}

</style>