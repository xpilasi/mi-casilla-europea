<script>
import { ChevronDown, User, ShoppingBag, ShoppingCart, Calculator, Menu, X } from 'lucide-vue-next';
export default {
  name: 'NavBar',
  components: {
    ChevronDown,
    User,
    ShoppingBag,
    ShoppingCart,
    Calculator,
    Menu,
    X
  },
  data() {
    return {
      classOption: 'block px-4 py-2 text-gray-800 no-underline hover:bg-gray-100',
      targetDate: new Date('2025-01-30T00:00:00'), // Set your target date here
      countdown: {
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0
      },
      options: [
      {
          title: 'INICIO',
          items: [''],
          options: false,
          isOpen: false
        },
        {

          title: 'TIENDAS',
          items: [['Tiendas de outlet','/1'], ['Tiendas ecológicas','/2'], ['Deporte','/3']],
          options: true,
          isOpen: false
        },
        
        {
          title: 'SERVICIOS',
          items: ['Item 3.1', 'Item 3.2', 'Item 3.3'],
          options: false,
          isOpen: false
        },
        {
          title: 'PRECIOS',
          items: ['Item 3.1', 'Item 3.2', 'Item 3.3'],
          options: false,
          isOpen: false
        },
        {
          title: 'HERRAMIENTAS',
          items: [['Regístrate o ingresa','/1'], ['Calculadora','/2'], ['Verificar franquicia','/3']],
          options: true,
          isOpen: false
        },
        {
          title: 'CONTACTO',
          items: ['Item 3.1', 'Item 3.2', 'Item 3.3'],
          options: false,
          isOpen: false
        },
      ],
      isMobileMenuOpen: false
    }
  },
  mounted() {
    this.updateCountdown();
    setInterval(this.updateCountdown, 1000);
    // Add window resize listener
    window.addEventListener('resize', this.handleResize);
  },
  unmounted() {
    // Clean up the event listener
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    handleResize() {
      // Close mobile menu and reset all option states when screen size changes
      if (window.innerWidth >= 1024) { // 1024px is the 'lg' breakpoint in Tailwind
        this.isMobileMenuOpen = false;
        this.options.forEach(option => {
          option.isOpen = false;
        });
      }
    },
    updateCountdown() {
      const now = new Date().getTime();
      const distance = this.targetDate - now;

      this.countdown.days = Math.floor(distance / (1000 * 60 * 60 * 24));
      this.countdown.hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      this.countdown.minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      this.countdown.seconds = Math.floor((distance % (1000 * 60)) / 1000);
    },
    toggleMobileMenu() {
      this.isMobileMenuOpen = !this.isMobileMenuOpen;
    }
  }
}
</script>
<template>
  <nav class="flex flex-col text-sm text-gray-500 fixed top-0 shadow-sm w-full lg:px-36 px-5 pt-5 bg-white">
    <div class="flex flex-row w-full justify-center text-xs">
      El vuelo cierra en <span class="text-white bg-customBlue px-1 mx-1">{{ countdown.days }}</span>días <span class="text-white bg-customBlue px-1 mx-1">{{ countdown.hours }}</span>horas <span class="text-white bg-customBlue px-1 mx-1">{{ countdown.minutes }}</span>minutos <span class="text-white bg-customBlue px-1 mx-1">{{ countdown.seconds }}</span>segundos
    </div>
   
    <div class="flex flex-row justify-between gap-2 mb-2">

      <!-- Logo -->
      <div class="flex flex-row justify-start w-54 ">
      <img src="./assets/img/logos/logo-mi-casilla-europea.png" alt="">
      </div>

      <!-- Buttons -->
      <div class="flex flex-row justify-end gap-2 items-end">

        <div class="flex flex-row justify-center items-center p-2 bg-customBlue text-white rounded-md cursor-pointer  hover:bg-blue-500 transition ease-out duration-200">
          <a class=" " href=""><User /></a>
        </div>
        <div class="flex flex-row justify-center items-center p-2 bg-customBlue text-white rounded-md cursor-pointer  hover:bg-blue-500 transition ease-out duration-200">
          <a class=" " href=""><Calculator /></a>
        </div>
        <div class="flex flex-row justify-center items-center p-2 bg-customBlue text-white rounded-md cursor-pointer  hover:bg-blue-500 transition ease-out duration-200">
          <a class=" " href=""><ShoppingCart /></a>
        </div>
        <div class="flex flex-row justify-center items-center p-2 bg-customBlue text-white rounded-md cursor-pointer  hover:bg-blue-500 transition ease-out duration-200">
          <a class=" " href=""><ShoppingBag /></a>
        </div>
      </div>
    </div>

    <!-- Hamburger Menu Button (visible on small screens) -->
    <div class="lg:hidden flex items-center">
      <button @click="toggleMobileMenu" class="text-customBlue p-2">
        <Menu v-if="!isMobileMenuOpen" class="h-6 w-6" />
        <X v-else class="h-6 w-6" />
      </button>
    </div>

    <!-- Menu Big screen (hidden on small screens) -->
    <div class="hidden lg:block flex text-xs flex-row justify-end text-customBlue gap-1 w-full flex-nowrap whitespace-nowrap">
    <div v-for="(option, index) in options" 
         :key="index" 
         class="relative inline-block"
         @mouseenter="option.isOpen = true"
         @mouseleave="option.isOpen = false">
      <button class="px-4 py-2  cursor-pointer  rounded-md rounded-b-none font-bold flex flex-row hover:bg-customBlue hover:text-white">
        {{ option.title }}
        <div v-if="option.options" class="ml-2">

          <ChevronDown />
        </div>
      </button>
      <transition
        enter-active-class="transition ease-out duration-200"
        enter-from-class="transform opacity-0 scale-95"
        enter-to-class="transform opacity-100 scale-100"
        leave-active-class="transition ease-in duration-150"
        leave-from-class="transform opacity-100 scale-100"
        leave-to-class="transform opacity-0 scale-95"
      >
        <div v-if="option.isOpen && option.options" 
             class="absolute top-full left-0 min-w-[150px] bg-customBlue rounded-md rounded-tl-none shadow-lg shadow-gray-300">
             <div class="bg-white w-full mt-1">

               <a v-for="(item, itemIndex) in option.items" 
                  :key="itemIndex" 
                  :href="item[1]" 
                  class="block px-4 py-2 text-gray-400 text-sm hover:bg-gray-100 w-full">
                 {{ item[0] }}
               </a>
             </div>
        </div>
      </transition>
    </div>
  </div>

    <!-- Mobile Menu -->
    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="transform -translate-y-full opacity-0"
      enter-to-class="transform translate-y-0 opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="transform translate-y-0 opacity-100"
      leave-to-class="transform -translate-y-full opacity-0"
    >
      <div v-if="isMobileMenuOpen" class="lg:hidden absolute top-full left-0 right-0 bg-white shadow-lg z-50">
        <div class="px-4 py-2">
          <div v-for="(option, index) in options" :key="index" class="border-b border-gray-200">
            <div 
              @click="option.isOpen = !option.isOpen"
              class="flex justify-between items-center py-3 cursor-pointer text-customBlue font-bold"
            >
              {{ option.title }}
              <ChevronDown 
                v-if="option.options"
                :class="{ 'transform rotate-180': option.isOpen }"
                class="transition-transform duration-200"
              />
            </div>
            <div 
              v-if="option.options && option.isOpen"
              class="py-2 pl-4"
            >
              <a
                v-for="(item, itemIndex) in option.items"
                :key="itemIndex"
                :href="item[1]"
                class="block py-2 text-gray-600 hover:text-customBlue"
              >
                {{ item[0] }}
              </a>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </nav>
  
</template>


