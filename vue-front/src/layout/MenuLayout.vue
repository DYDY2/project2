<template>
    <div class="collapse navbar-collapse" ref="sidebar">
                <div class="container-fluid">
                    <div class="row">
                        <!-- 사이드 메뉴 -->
                        <div id="sidebar-menu" class="col-lg-2 sidebar fixed-top" :style="sidebar" :class="{'off-On': offOn}">
                            <div class="navbar-brand text-center d-block mx-auto py-3 mb-3 bottom-border"></div>
                            <div class="bottom-border pb-3 d-flex" v-if="this.user.user_no">
                                <i class="bi bi-person-circle mx-3 fs-4-0"></i>
                                <a href="#" class="text-white" @click="goToMypage">{{ this.user.user_id }}</a>
                                <p style="padding: 0; margin: 0;">({{ this.adminCheck }})</p>
                            </div>
                            <ul class="navbar-nav flex-column mt-2" style="text-align:left;">
                                <!-- 홈 -->
                                <!-- 한자 -->
                                <li class="nav-item">
                                    <a href="#Han" class="nav-link text-white p-3 mb-2 sidebar-link" data-bs-toggle="collapse" aria-expanded="false" aria-controls="Han" @click="categoryClick(3)">
                                        <i class="bi bi-alipay"></i>한자
                                    </a>
                                    <div class="collapse" id="Han">
                                        <ul class="navbar-nav text-white flex-column ms-4">
                                            <li class="nav-item" @click="changeLevel(3, 1)"><a href="#" class="nav-link">초급</a></li>
                                            <li class="nav-item" @click="changeLevel(3, 2)"><a href="#" class="nav-link">중급</a></li>
                                            <li class="nav-item" @click="changeLevel(3, 3)"><a href="#" class="nav-link">고급</a></li>
                                        </ul>
                                    </div>
                                </li>
                                <!-- 영어 -->
                                <li class="nav-item">
                                    <a href="#Eng" class="nav-link text-white p-3 mb-2 sidebar-link" data-bs-toggle="collapse" aria-controls="Eng" aria-expanded="false" @click="categoryClick(2)">
                                        <i class="bi bi-alipay"></i>영어
                                    </a>
                                    <div class="collapse" id="Eng">
                                        <ul class="navbar-nav text-white flex-column ms-4">
                                            <li class="nav-item" @click="changeLevel(2, 1)"><a href="#" class="nav-link">초급</a></li>
                                            <li class="nav-item" @click="changeLevel(2, 2)"><a href="#" class="nav-link">중급</a></li>
                                            <li class="nav-item"  @click="changeLevel(2, 3)"><a href="#" class="nav-link">고급</a></li>
                                        </ul>
                                    </div>
                                </li>
                                <!-- 수학 -->
                                <li class="nav-item">
                                    <a href="#Math" class="nav-link text-white p-3 mb-2 sidebar-link" data-bs-toggle="collapse" aria-controls="Math" aria-expanded="false" @click="categoryClick(1)">
                                        <i class="bi bi-alipay"></i>수학
                                    </a>
                                    <div class="collapse" id="Math">
                                        <ul class="navbar-nav text-white flex-column ms-4">
                                            <li class="nav-item" @click="changeLevel(1, 1)"><a href="#" class="nav-link">초급</a></li>
                                            <li class="nav-item" @click="changeLevel(1, 2)"><a href="#" class="nav-link">중급</a></li>
                                            <li class="nav-item" @click="changeLevel(1, 3)"><a href="#" class="nav-link">고급</a></li>
                                        </ul>
                                    </div>
                                </li>
                                <!-- 국어 -->
                                <li class="nav-item">
                                    <a href="#Lan" class="nav-link text-white p-3 mb-2 sidebar-link" data-bs-toggle="collapse" aria-controls="Lan" aria-expanded="false" @click="categoryClick(4)">
                                        <i class="bi bi-alipay"></i>국어
                                    </a>
                                    <div class="collapse" id="Lan">
                                        <ul class="navbar-nav text-white flex-column ms-4">
                                            <li class="nav-item" @click="changeLevel(4, 1)"><a href="#" class="nav-link">초급</a></li>
                                            <li class="nav-item" @click="changeLevel(4, 2)"><a href="#" class="nav-link">중급</a></li>
                                            <li class="nav-item" @click="changeLevel(4, 3)"><a href="#" class="nav-link">고급</a></li>
                                        </ul>
                                    </div>
                                </li>
                                <li class="nav-item">
                                    <a href="#" class="nav-link text-white p-3 mb-2 sidebar-link" @click="goToBoard">
                                        <i class="bi bi-alipay"></i>자유게시판
                                    </a>
                                </li>
                                <li class="nav-item">
                                    <a href="#" class="nav-link text-white p-3 mb-2 sidebar-link" @click="goToQna">
                                        <i class="bi bi-alipay"></i>QnA
                                    </a>
                                </li>
                                <li class="nav-item">
                                    <a href="#" class="nav-link text-white p-3 mb-2 sidebar-link" @click="goToNotice">
                                        <i class="bi bi-alipay"></i>공지사항
                                    </a>
                                </li>
                            </ul>
                        </div>
                        <!-- /사이드 메뉴 -->       
                </div>
            </div>
        </div>
</template>

<script>

export default {
    data() {
        return {
            offOn: false,
            quizLevel: {
                category: {},
                level: {},
            },
            sidebarTop: '0',
            menuCategory: '',
            menuLevel: '',
            width: window.innerWidth,
            adminCheck: '',
            // adminValue: '',

        }
}, 
    created() {
    },
    mounted() {
        this.emitter.on('SideBarMenu', this.toggleMenu);
        this.$router.afterEach((to) => {
                        console.log('현재 주소:', to.path);
                        if(to.path) {
                        // this.$store.commit('setCurrentUrl', '');
                        this.$store.commit('setCurrentUrl', to.path);
                            
                        }
                 });
         this.userAdminCheck();
        window.addEventListener('resize', this.updateWidth);
    },
    beforeUnmount() {
        this.emitter.off('SideBarMenu', this.toggleMenu);
        window.removeEventListener('resize', this.updateWidth);
    },
    computed: {
        user() {
            return this.$store.state.user;
        },

        headerHeight() {
            return this.$store.state.headerHeight;
        },
        baseTop() {
            return parseInt(this.sidebarTop || '0', 10);
            
        },
        naviHeight() {
            return this.$store.state.naviHeight;
        },
        computedTop() {
            const height = this.headerHeight;
            const naviHeight = this.naviHeight
            const baseTop = this.baseTop;
            if (!isNaN(height) && !isNaN(baseTop) && !isNaN(naviHeight)) {
                
                return baseTop +(height + naviHeight);

            }

            return baseTop;
        },
        sidebar() {
            return {
                top: `${this.computedTop}px`,
                // marginLeft: this.$store.state.sidebarMarginLeft,
                marginLeft: this.computedMarginLeft,
                transition: 'margin 0.25s ease-out',
            };
        },
        setUpUrl() {
            return this.$store.state.currentUrl;
        },
        computedMarginLeft() {
            if (this.width >= 1024) {
                const marginLeft = this.offOn ? '0' : '-30rem';
                this.$store.commit('setSidebarMarginLeft', marginLeft); 
                return marginLeft;
            }
            const marginLeft = this.offOn ? '0' : '-100vw';
            this.$store.commit('setSidebarMarginLeft', marginLeft);
            return marginLeft;
        },
        adminCk() {
            return this.$store.state.adminCk;
        },
       

    },
    methods: {
        //head에서 toggled시 메뉴가 나온다
        toggleMenu() {
            this.offOn = !this.offOn;
            // const newMarginLeft = this.offOn ? '0' : '-30rem';
            this.$store.commit('setSidebarMarginLeft', this.computedMarginLeft);
            this.emitter.emit('sidebar-toggled', this.offOn ? 'open' : 'closed');
        },
        //화면 해상도
        updateWidth() {
            this.width = window.innerWidth;
            this.$store.commit('setWindoWith', this.width)
        },

        goToQna() {
            this.$store.commit('setQnaNumber', 7);
            // return window.location.href = 'http://localhost:8080/qna?page=1'
            return this.$router.push({path: `/qna`, query:{page : 1}});
        },


        categoryClick(category) {
            this.menuCategory = category
        },

        async changeLevel(category, level) {

            if(this.menuCategory !== category) {

                this.menuCategory = category
            }

            try {

                if(this.menuCategory) {
                    const path = `/quiz/${this.menuCategory}/${level}`;
                    if(this.isMounted) {
                    console.log(this.menuCategory);
                    console.log(level);

                    } else {
                        
                    console.log('created:',this.menuCategory);
                    console.log('created:',level);
                    }
                    await this.$router.push(path);
                }
            } catch(error) {
                console.log('error:',)
            }
        },
        goToBoard() {
            this.$store.commit('setBoardNumber', 5);
                //  return window.location.href = 'http://localhost:8080/board?page=1'
            return this.$router.push({path: `/board?page=1`});
        },
        goToNotice() {
            this.$store.commit('setNoticeNumber', 6);
            // return window.location.href = `http://localhost:8080/notice`
            return this.$router.push({path: `/notice`});
        },
        userAdminCheck() {

            const admin = this.adminCk;
            if(admin == '1') {
                this.adminCheck = '관리자'
                
            } else {
                this.adminCheck = '사용자'
            }
            return this.adminCheck;

        },
        goToMypage() {
            this.$router.push({path: '/mypage'});
        }


    }
}
</script>
<style scoped>
.sidebar {
    height: 100vh;
    background: rgba(174, 190, 174, 0.7);
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    box-shadow: 2px 7px 9px #b9b9b9;
}

.bottom-border {
        border-bottom: 2px groove #eee;
    }

    .sidebar-link{
        transition: all .4s
    }

    .sidebar-link:hover {
        background-color: #83e980;
        border-radius: 5px;
    }

    .sidebar-link:visited {
        background-color: #83e980;
        border-radius: 5px;
    }
    

    .bi-house {
        margin-right: 0.2rem;
    }

    .bi-alipay {
        margin-right: 0.2rem;
    }

    @media (min-width: 969px) {

    }

</style>