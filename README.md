import React, { useState, useEffect, useMemo } from 'react';
import { 
  LayoutDashboard, 
  ShoppingCart, 
  Package, 
  Users, 
  FileText, 
  Settings, 
  Search, 
  Plus, 
  Trash2, 
  Edit, 
  Menu, 
  X, 
  ChevronRight, 
  DollarSign,
  CreditCard,
  LogOut,
  RefreshCw,
  AlertCircle,
  TrendingUp,
  Wallet,
  ArrowDownLeft,
  ArrowUpRight,
  Printer,
  Truck,
  TicketPercent,
  MessageSquare,
  Smartphone,
  Send,
  Languages,
  History,
  AlertTriangle,
  Clock,
  BarChart3,
  ArrowRightLeft,
  Hash,
  FileBarChart,
  ClipboardList,
  Upload,
  Calendar,
  ScanLine,
  User,
  MapPin,
  Phone,
  Download,
  HelpCircle,
  ChevronDown,
  ChevronUp,
  LifeBuoy,
  File,
  Globe,
  Copy,
  Check,
  ExternalLink,
  RefreshCcw,
  Repeat
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInAnonymously, 
  signInWithCustomToken, 
  onAuthStateChanged, 
  signOut
} from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  addDoc, 
  updateDoc, 
  deleteDoc, 
  doc, 
  onSnapshot, 
  serverTimestamp, 
  query, 
  writeBatch,
  increment,
  where,
  orderBy,
  limit
} from 'firebase/firestore';

// --- Firebase Config ---
const firebaseConfig = JSON.parse(__firebase_config || '{}');
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';

// --- Translations ---
const TRANSLATIONS = {
  en: {
    dashboard: 'Dashboard',
    sales: 'Sales',
    delivery: 'Delivery',
    marketing: 'Marketing',
    purchase: 'Purchase',
    expenses: 'Expenses',
    stock: 'Inventory',
    parties: 'Parties',
    reports: 'Reports',
    settings: 'Settings',
    home: 'Home',
    promo_sms: 'Promo & SMS',
    courier: 'Courier',
    inventory: 'Inventory',
    todays_overview: "Today's Overview",
    total_sales: "Total Sales",
    total_expense: "Total Expense",
    net_profit: "Net Profit",
    pending_delivery: "Pending Delivery",
    new_sale: "New Sale",
    bulk_sms: "Bulk SMS",
    search_product: "Search Product...",
    bill: "Bill",
    items: "Items",
    total: "Total",
    checkout: "Checkout",
    finalize_sale: "Finalize Sale",
    customer: "Customer",
    walk_in: "Walk-in Customer",
    coupon_code: "Coupon Code",
    apply: "Apply",
    store_pickup: "Store Pickup",
    courier_delivery: "Courier Delivery",
    payment_method: "Payment Method",
    subtotal: "Subtotal",
    discount: "Discount",
    delivery_fee: "Delivery Fee",
    total_payable: "Total Payable",
    paid_amount: "Paid Amount",
    due: "Due",
    confirm_sale: "Confirm Sale",
    delivery_tracking: "Delivery Tracking",
    order_id: "Order ID",
    tracking: "Tracking",
    status: "Status",
    coupon_management: "Coupon Management",
    bulk_sms_marketing: "Bulk SMS Marketing",
    send_broadcast: "Send Broadcast",
    sales_report_invoice: "Sales Report & Invoice",
    date: "Date",
    amount: "Amount",
    method: "Method",
    action: "Action",
    invoice: "Invoice",
    add_expense: "Add Expense",
    note: "Note",
    category: "Category",
    product_stock: "Product Stock",
    add_product: "Add Product",
    name: "Name",
    cost: "Costing Price",
    price: "MRP",
    party_list: "Party List",
    add_party: "Add Party",
    suppliers: "Suppliers",
    purchase_stock: "Purchase Stock",
    supplier: "Supplier",
    product: "Product",
    qty: "Qty",
    unit_cost: "Unit Cost",
    confirm_purchase: "Confirm Purchase",
    shop_config: "Shop Configuration",
    shop_name: "Shop Name",
    currency: "Currency",
    save_changes: "Save Changes",
    log_out: "Log Out",
    owner: "Owner",
    business_suite: "Business Suite",
    pending: "Pending",
    shipped: "Shipped",
    delivered: "Delivered",
    type_message: "Type your message here...",
    add: "Add",
    close: "Close",
    print_invoice: "Print Invoice",
    bill_to: "Bill To",
    via: "Via",
    history: "History",
    stock_log: "Stock Log",
    low_stock: "Low Stock",
    adjustment: "Adjustment",
    purchase_history: "Purchase History",
    total_spent: "Total Spent",
    view_history: "View History",
    inv_dashboard: "Inventory Dashboard",
    product_list: "Product List",
    stock_report: "Stock Report",
    detailed_report: "Detailed Report",
    stock_ledger: "Stock Ledger",
    stock_transfer: "Stock Transfer",
    serial_manage: "Serial Manager",
    total_items: "Total Items",
    stock_value_cost: "Stock Value (Cost)",
    stock_value_sales: "Stock Value (Sales)",
    est_profit: "Est. Profit",
    transfer: "Transfer",
    from: "From",
    to: "To",
    serial_no: "Serial No",
    add_serial: "Add Serial",
    available: "Available",
    sold: "Sold",
    brand: "Brand",
    units: "Units",
    size: "Size",
    color: "Color",
    wholesale_price: "Wholesale Price",
    vat: "VAT",
    is_vat: "Is Vat Applicable?",
    alert_qty: "Stock Alert Quantity",
    stock_in: "Stock In",
    warranty: "Warranty",
    mfg_date: "Manufacturing Date",
    exp_date: "Expiry Date",
    upload_photo: "Upload Photo",
    variations: "Variations",
    sku: "SKU",
    create_invoice: "Create Invoice",
    sold_history: "Sold History",
    sold_products: "Sold Products",
    customer_history: "Customer History",
    retail_sale: "Retail Sale",
    wholesale: "Wholesale",
    phone_number: "Phone Number",
    customer_name: "Customer Name",
    address: "Address",
    total_qty: "Total Qty",
    additional_expense: "Additional Expense",
    total_discount: "Total Discount",
    change_amount: "Change Amount",
    remarks: "Remarks",
    service_staff: "Service Staff",
    sl: "SL",
    invoice_no: "Invoice No",
    type: "Type",
    quantity: "Quantity",
    store_name: "Store Name",
    faq: "FAQ",
    help: "Help",
    faq_title: "Frequently Asked Questions",
    faq_subtitle: "Find answers to commonly asked questions",
    q1: "How can I reset my password?",
    q2: "Do you offer a free trial?",
    q3: "Can I change my subscription plan later?",
    q4: "Is my data secure?",
    q5: "Do you provide customer support?",
    support: "Help & Support",
    ticket_list: "Ticket List",
    create_ticket: "Create Ticket",
    hotline: "Hotline",
    submit_ticket: "Submit a Ticket",
    priority: "Priority",
    subject: "Subject",
    message: "Message",
    upload_files: "Upload Files",
    drag_drop: "Drag files here or click to browse",
    max_size: "Max size 5MB each, supported: images/text/pdf",
    submit_btn: "Submit Ticket",
    ticket_id: "Ticket ID",
    community: "Community",
    join_community: "Join Our Community",
    social_links: "Social Links",
    copy_link: "Copy Link",
    copied: "Copied!",
    visit: "Visit",
    return_exchange: "Return & Exchange",
    return: "Return",
    exchange: "Exchange",
    sales_return: "Sales Return",
    return_history: "Return History",
    return_products: "Return Products",
    sales_exchange: "Sales Exchange",
    exchange_history: "Exchange History",
    exchange_products: "Exchange Products",
    billing_summary: "Billing Summary",
    retail_return: "Retail Return",
    party_return: "Party Return",
    total_deduction: "Total Deduction",
    total_returnable: "Total Returnable",
    return_qty: "Return Qty",
    return_value: "Return Value",
    exchange_qty: "Exchange Qty",
    exchange_value: "Exchange Value",
    payable_amount: "Payable Amount",
    submit_exchange: "Submit Exchange",
    submit_return: "Submit Return"
  },
  bn: {
    dashboard: 'ড্যাশবোর্ড',
    sales: 'বিক্রয়',
    delivery: 'ডেলিভারি',
    marketing: 'মার্কেটিং',
    purchase: 'ক্রয়',
    expenses: 'খরচ',
    stock: 'ইনভেন্টরি',
    parties: 'পার্টি',
    reports: 'রিপোর্ট',
    settings: 'সেটিংস',
    home: 'হোম',
    promo_sms: 'প্রোমো ও এসএমএস',
    courier: 'কুরিয়ার',
    inventory: 'ইনভেন্টরি',
    todays_overview: "আজকের ওভারভিউ",
    total_sales: "মোট বিক্রয়",
    total_expense: "মোট খরচ",
    net_profit: "নিট লাভ",
    pending_delivery: "পেন্ডিং ডেলিভারি",
    new_sale: "নতুন বিক্রয়",
    bulk_sms: "বাল্ক SMS",
    search_product: "পণ্য খুঁজুন...",
    bill: "বিল",
    items: "আইটেম",
    total: "মোট",
    checkout: "চেকআউট",
    finalize_sale: "বিক্রয় সম্পন্ন করুন",
    customer: "গ্রাহক",
    walk_in: "সাধারণ গ্রাহক",
    coupon_code: "কুপন কোড",
    apply: "প্রয়োগ করুন",
    store_pickup: "দোকান থেকে সংগ্রহ",
    courier_delivery: "কুরিয়ার ডেলিভারি",
    payment_method: "পেমেন্ট মেথড",
    subtotal: "সাবটোটাল",
    discount: "ডিসকাউন্ট",
    delivery_fee: "ডেলিভারি ফি",
    total_payable: "মোট প্রদেয়",
    paid_amount: "জমা দিয়েছেন",
    due: "বাকি",
    confirm_sale: "বিক্রয় নিশ্চিত করুন",
    delivery_tracking: "ডেলিভারি ট্র্যাকিং",
    order_id: "অর্ডার আইডি",
    tracking: "ট্র্যাকিং",
    status: "স্ট্যাটাস",
    coupon_management: "কুপন ম্যানেজমেন্ট",
    bulk_sms_marketing: "বাল্ক SMS মার্কেটিং",
    send_broadcast: "ব্রডকাস্ট পাঠান",
    sales_report_invoice: "বিক্রয় রিপোর্ট ও ইনভয়েস",
    date: "তারিখ",
    amount: "পরিমাণ",
    method: "মাধ্যম",
    action: "অ্যাকশন",
    invoice: "ইনভয়েস",
    add_expense: "খরচ যোগ করুন",
    note: "বিবরণ",
    category: "ক্যাটাগরি",
    product_stock: "পণ্যের স্টক",
    add_product: "পণ্য যোগ করুন",
    name: "নাম",
    cost: "ক্রয় মূল্য",
    price: "খুচরা মূল্য (MRP)",
    party_list: "পার্টি তালিকা",
    add_party: "পার্টি যোগ করুন",
    suppliers: "সরবরাহকারী",
    purchase_stock: "পণ্য ক্রয়",
    supplier: "সরবরাহকারী",
    product: "পণ্য",
    qty: "পরিমাণ",
    unit_cost: "ইউনিট খরচ",
    confirm_purchase: "ক্রয় নিশ্চিত করুন",
    shop_config: "দোকান কনফিগারেশন",
    shop_name: "দোকানের নাম",
    currency: "মুদ্রা",
    save_changes: "পরিবর্তন সংরক্ষণ করুন",
    log_out: "লগ আউট",
    owner: "দোকান মালিক",
    business_suite: "বিজনেস স্যুট",
    pending: "পেন্ডিং",
    shipped: "শিপড",
    delivered: "ডেলিভারড",
    type_message: "আপনার বার্তা লিখুন...",
    add: "যোগ করুন",
    close: "বন্ধ করুন",
    print_invoice: "প্রিন্ট ইনভয়েস",
    bill_to: "বিল টু",
    via: "মাধ্যম",
    history: "হিস্ট্রি",
    stock_log: "স্টক লগ",
    low_stock: "লো স্টক",
    adjustment: "সমন্বয়",
    purchase_history: "ক্রয় ইতিহাস",
    total_spent: "মোট খরচ",
    view_history: "হিস্ট্রি দেখুন",
    inv_dashboard: "ইনভেন্টরি ড্যাশবোর্ড",
    product_list: "পণ্য তালিকা",
    stock_report: "স্টক রিপোর্ট",
    detailed_report: "বিস্তারিত রিপোর্ট",
    stock_ledger: "স্টক লেজার",
    stock_transfer: "স্টক ট্রান্সফার",
    serial_manage: "সিরিয়াল ম্যানেজ",
    total_items: "মোট আইটেম",
    stock_value_cost: "স্টক মান (ক্রয়)",
    stock_value_sales: "স্টক মান (বিক্রয়)",
    est_profit: "সম্ভাব্য লাভ",
    transfer: "ট্রান্সফার",
    from: "থেকে",
    to: "প্রতি",
    serial_no: "সিরিয়াল নং",
    add_serial: "সিরিয়াল যোগ",
    available: "মজুদ",
    sold: "বিক্রিত",
    brand: "ব্র্যান্ড",
    units: "ইউনিট",
    size: "সাইজ",
    color: "রঙ",
    wholesale_price: "পাইকারি মূল্য",
    vat: "ভ্যাট",
    is_vat: "ভ্যাট প্রযোজ্য?",
    alert_qty: "স্টক অ্যালার্ট পরিমাণ",
    stock_in: "স্টক ইন",
    warranty: "ওয়ারেন্টি",
    mfg_date: "উৎপাদন তারিখ",
    exp_date: "মেয়াদ উত্তীর্ণের তারিখ",
    upload_photo: "ছবি আপলোড",
    variations: "ভেরিয়েশন",
    sku: "SKU",
    create_invoice: "ইনভয়েস তৈরি",
    sold_history: "বিক্রয় ইতিহাস",
    sold_products: "বিক্রিত পণ্য",
    customer_history: "গ্রাহক ইতিহাস",
    retail_sale: "খুচরা বিক্রয়",
    wholesale: "পাইকারি",
    phone_number: "ফোন নম্বর",
    customer_name: "গ্রাহকের নাম",
    address: "ঠিকানা",
    total_qty: "মোট পরিমাণ",
    additional_expense: "অতিরিক্ত খরচ",
    total_discount: "মোট ডিসকাউন্ট",
    change_amount: "ফেরত পরিমাণ",
    remarks: "মন্তব্য",
    service_staff: "সার্ভিস স্টাফ",
    sl: "SL",
    invoice_no: "ইনভয়েস নং",
    type: "ধরন",
    quantity: "পরিমাণ",
    store_name: "দোকানের নাম",
    faq: "জিজ্ঞাসিত প্রশ্ন",
    help: "হেল্প",
    faq_title: "সচরাচর জিজ্ঞাসিত প্রশ্নাবলী",
    faq_subtitle: "সাধারণ প্রশ্নের উত্তর খুঁজুন",
    q1: "আমি কিভাবে আমার পাসওয়ার্ড রিসেট করব?",
    q2: "আপনারা কি ফ্রি ট্রায়াল দেন?",
    q3: "আমি কি পরে সাবস্ক্রিপশন প্ল্যান পরিবর্তন করতে পারব?",
    q4: "আমার ডেটা কি সুরক্ষিত?",
    q5: "আপনারা কি কাস্টমার সাপোর্ট দেন?",
    support: "হেল্প ও সাপোর্ট",
    ticket_list: "টিকেট লিস্ট",
    create_ticket: "টিকেট তৈরি করুন",
    hotline: "হটলাইন",
    submit_ticket: "টিকেট জমা দিন",
    priority: "অগ্রাধিকার",
    subject: "বিষয়",
    message: "বার্তা",
    upload_files: "ফাইল আপলোড",
    drag_drop: "ফাইল এখানে টেনে আনুন বা ব্রাউজ করুন",
    max_size: "সর্বোচ্চ ৫ মেগাবাইট, ইমেজ/টেক্সট/পিডিএফ",
    submit_btn: "টিকেট জমা দিন",
    ticket_id: "টিকেট আইডি",
    community: "কমিউনিটি",
    join_community: "কমিউনিটিতে যোগ দিন",
    social_links: "সোশ্যাল লিংক",
    copy_link: "লিংক কপি",
    copied: "কপি হয়েছে!",
    visit: "ভিজিট",
    return_exchange: "রিটার্ন ও এক্সচেঞ্জ",
    return: "রিটার্ন",
    exchange: "এক্সচেঞ্জ",
    sales_return: "সেলস রিটার্ন",
    return_history: "রিটার্ন হিস্ট্রি",
    return_products: "রিটার্ন পণ্য",
    sales_exchange: "সেলস এক্সচেঞ্জ",
    exchange_history: "এক্সচেঞ্জ হিস্ট্রি",
    exchange_products: "এক্সচেঞ্জ পণ্য",
    billing_summary: "বুকিং সারসংক্ষেপ",
    retail_return: "খুচরা রিটার্ন",
    party_return: "পার্টি রিটার্ন",
    total_deduction: "মোট কর্তন",
    total_returnable: "মোট ফেরতযোগ্য",
    return_qty: "রিটার্ন পরিমাণ",
    return_value: "রিটার্ন মূল্য",
    exchange_qty: "এক্সচেঞ্জ পরিমাণ",
    exchange_value: "এক্সচেঞ্জ মূল্য",
    payable_amount: "প্রদেয় পরিমাণ",
    submit_exchange: "এক্সচেঞ্জ নিশ্চিত করুন",
    submit_return: "রিটার্ন নিশ্চিত করুন"
  }
};

// --- Helper Components ---
const Card = ({ children, className = "" }) => (
  <div className={`bg-white rounded-xl shadow-sm border border-slate-200 ${className}`}>
    {children}
  </div>
);

const Button = ({ children, onClick, variant = 'primary', className = "", disabled = false, icon: Icon = null }) => {
  const baseStyle = "px-4 py-2.5 rounded-lg font-medium transition-all flex items-center justify-center gap-2 active:scale-95 disabled:opacity-50 disabled:active:scale-100";
  const variants = {
    primary: "bg-blue-600 text-white hover:bg-blue-700 shadow-sm",
    secondary: "bg-slate-100 text-slate-700 hover:bg-slate-200",
    danger: "bg-red-50 text-red-600 hover:bg-red-100",
    success: "bg-emerald-600 text-white hover:bg-emerald-700",
    outline: "border border-slate-300 text-slate-600 hover:bg-slate-50"
  };
  return (
    <button onClick={onClick} disabled={disabled} className={`${baseStyle} ${variants[variant]} ${className}`}>
      {Icon && <Icon size={18} />}
      {children}
    </button>
  );
};

const Modal = ({ title, onClose, children, size = 'md' }) => (
  <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4 animate-in fade-in duration-200">
    <Card className={`w-full ${size === 'lg' ? 'max-w-4xl' : 'max-w-lg'} max-h-[90vh] overflow-hidden flex flex-col bg-white`}>
       <div className="p-4 border-b flex justify-between items-center bg-slate-50">
         <h3 className="font-bold text-lg text-slate-800">{title}</h3>
         <button onClick={onClose} className="p-1 hover:bg-slate-200 rounded-full transition-colors"><X className="text-slate-500" size={20}/></button>
       </div>
       <div className="overflow-y-auto p-4 flex-1">
         {children}
       </div>
    </Card>
  </div>
);

// --- Main App ---
export default function App() {
  const [user, setUser] = useState(null);
  const [activeTab, setActiveTab] = useState('dashboard');
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [loading, setLoading] = useState(true);
  const [lang, setLang] = useState('bn');

  useEffect(() => {
    const initAuth = async () => {
      if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
        await signInWithCustomToken(auth, __initial_auth_token);
      } else {
        await signInAnonymously(auth);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (u) => {
      setUser(u);
      setLoading(false);
    });
    return () => unsubscribe();
  }, []);

  const handleLogout = () => signOut(auth);
  const t = (key) => TRANSLATIONS[lang][key] || key;

  if (loading) return <div className="h-screen flex items-center justify-center bg-slate-50 text-blue-600"><RefreshCw className="animate-spin w-8 h-8"/></div>;

  const menuItems = [
    { id: 'dashboard', icon: LayoutDashboard, label: t('dashboard'), sub: t('home') },
    { id: 'sales', icon: ShoppingCart, label: t('sales'), sub: t('new_sale') },
    { id: 'return_exchange', icon: ArrowRightLeft, label: t('return_exchange'), sub: t('return') + ' & ' + t('exchange') },
    { id: 'delivery', icon: Truck, label: t('delivery'), sub: t('courier') }, 
    { id: 'marketing', icon: TicketPercent, label: t('marketing'), sub: t('promo_sms') }, 
    { id: 'stock', icon: Package, label: t('stock'), sub: t('inventory') },
    { id: 'purchase', icon: ArrowDownLeft, label: t('purchase'), sub: t('inventory') },
    { id: 'expenses', icon: Wallet, label: t('expenses'), sub: t('add_expense') },
    { id: 'parties', icon: Users, label: t('parties'), sub: t('customer') },
    { id: 'reports', icon: FileText, label: t('reports'), sub: t('invoice') },
    { id: 'faq', icon: HelpCircle, label: t('faq'), sub: t('help') },
    { id: 'support', icon: LifeBuoy, label: t('support'), sub: t('ticket_list') },
    { id: 'community', icon: Globe, label: t('community'), sub: t('social_links') },
    { id: 'settings', icon: Settings, label: t('settings'), sub: t('shop_config') },
  ];

  return (
    <div className="flex h-screen bg-slate-50 text-slate-800 font-sans overflow-hidden">
      {/* Mobile Sidebar Overlay */}
      {isSidebarOpen && (
        <div className="fixed inset-0 bg-black/50 z-40 lg:hidden backdrop-blur-sm" onClick={() => setIsSidebarOpen(false)} />
      )}

      {/* Sidebar */}
      <aside className={`fixed lg:static inset-y-0 left-0 z-50 w-72 bg-white border-r border-slate-200 transform transition-transform duration-300 ease-out ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0'} flex flex-col shadow-2xl lg:shadow-none`}>
        <div className="p-6 border-b border-slate-100 flex items-center gap-3">
          <div className="bg-blue-600 p-2.5 rounded-xl shadow-lg shadow-blue-600/20">
            <TrendingUp className="w-6 h-6 text-white" />
          </div>
          <div className="flex-1">
            <h1 className="text-xl font-bold text-slate-900">SmartBiz</h1>
            <p className="text-xs text-slate-500">{t('business_suite')}</p>
          </div>
          <button 
            onClick={() => setLang(prev => prev === 'en' ? 'bn' : 'en')}
            className="p-1.5 rounded-lg bg-slate-100 text-slate-600 hover:bg-blue-50 hover:text-blue-600 font-bold text-xs border border-slate-200"
            title="Switch Language"
          >
            {lang === 'en' ? 'BN' : 'EN'}
          </button>
        </div>

        <nav className="flex-1 p-4 space-y-1 overflow-y-auto">
          {menuItems.map((item) => (
            <button
              key={item.id}
              onClick={() => { setActiveTab(item.id); setIsSidebarOpen(false); }}
              className={`w-full flex items-center gap-4 px-4 py-3 rounded-xl transition-all duration-200 group ${activeTab === item.id ? 'bg-blue-50 text-blue-700 font-semibold' : 'text-slate-600 hover:bg-slate-50'}`}
            >
              <item.icon className={`w-5 h-5 ${activeTab === item.id ? 'text-blue-600' : 'text-slate-400 group-hover:text-slate-600'}`} />
              <div className="text-left">
                <span className="block text-sm leading-none">{item.label}</span>
                <span className="text-[10px] text-slate-400 font-normal uppercase tracking-wider">{item.sub}</span>
              </div>
              {activeTab === item.id && <ChevronRight className="ml-auto w-4 h-4 text-blue-400" />}
            </button>
          ))}
        </nav>
        <div className="p-4 border-t border-slate-100 bg-slate-50/50">
           <div className="flex items-center gap-3">
             <div className="w-10 h-10 rounded-full bg-slate-200 flex items-center justify-center font-bold text-slate-600">{user?.isAnonymous ? 'G' : 'A'}</div>
             <div className="flex-1 min-w-0">
               <p className="text-sm font-medium text-slate-900 truncate">{t('owner')}</p>
               <button onClick={handleLogout} className="text-xs text-red-500 hover:underline">{t('log_out')}</button>
             </div>
           </div>
        </div>
      </aside>

      {/* Main Content */}
      <main className="flex-1 flex flex-col h-full overflow-hidden relative w-full">
        <header className="lg:hidden h-16 bg-white border-b border-slate-200 flex items-center justify-between px-4 sticky top-0 z-30">
          <button onClick={() => setIsSidebarOpen(true)} className="p-2 -ml-2 text-slate-600 active:bg-slate-100 rounded-lg"><Menu className="w-6 h-6" /></button>
          <span className="font-bold text-slate-800 text-lg">{menuItems.find(i => i.id === activeTab)?.label}</span>
          <div className="w-8" />
        </header>

        <div className="flex-1 overflow-y-auto overflow-x-hidden bg-slate-50/50">
          <div className="max-w-7xl mx-auto p-4 lg:p-8 min-h-full">
            {activeTab === 'dashboard' && <DashboardView user={user} appId={appId} setTab={setActiveTab} lang={lang} />}
            {activeTab === 'sales' && <SalesView user={user} appId={appId} lang={lang} />}
            {activeTab === 'return_exchange' && <ReturnExchangeView user={user} appId={appId} lang={lang} />}
            {activeTab === 'delivery' && <DeliveryView user={user} appId={appId} lang={lang} />}
            {activeTab === 'marketing' && <MarketingView user={user} appId={appId} lang={lang} />}
            {activeTab === 'purchase' && <PurchaseView user={user} appId={appId} lang={lang} />}
            {activeTab === 'expenses' && <ExpenseView user={user} appId={appId} lang={lang} />}
            {activeTab === 'stock' && <InventoryView user={user} appId={appId} lang={lang} />}
            {activeTab === 'parties' && <PartiesView user={user} appId={appId} lang={lang} />}
            {activeTab === 'reports' && <ReportsView user={user} appId={appId} lang={lang} />}
            {activeTab === 'faq' && <FAQView lang={lang} />}
            {activeTab === 'support' && <SupportView user={user} appId={appId} lang={lang} />}
            {activeTab === 'community' && <CommunityView lang={lang} />}
            {activeTab === 'settings' && <SettingsView user={user} appId={appId} lang={lang} />}
          </div>
        </div>
      </main>
    </div>
  );
}

// --- VIEWS ---

function DashboardView({ user, appId, setTab, lang }) {
  const [stats, setStats] = useState({ sales: 0, expenses: 0, profit: 0, due: 0, pendingDelivery: 0 });
  const t = (key) => TRANSLATIONS[lang][key] || key;

  useEffect(() => {
    if (!user) return;
    const qSales = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(qSales, (snap) => {
      let salesTotal = 0, expensesTotal = 0, totalDue = 0, pendingDelivery = 0;
      snap.forEach(doc => {
        const data = doc.data();
        if (data.type === 'sale') salesTotal += data.amount || 0;
        if (data.type === 'expense') expensesTotal += data.amount || 0;
        if (data.dueAmount > 0) totalDue += data.dueAmount || 0;
        if (data.deliveryStatus === 'Pending') pendingDelivery += 1;
      });
      setStats({ sales: salesTotal, expenses: expensesTotal, profit: salesTotal - expensesTotal, due: totalDue, pendingDelivery });
    });
    return () => unsub();
  }, [user, appId]);

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
         <h2 className="text-2xl font-bold text-slate-800">{t('todays_overview')}</h2>
      </div>
      <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
        <div className="bg-white p-4 rounded-xl shadow-sm border border-emerald-100"><p className="text-xs text-slate-500 uppercase">{t('total_sales')}</p><h3 className="text-2xl font-bold text-emerald-600">৳{stats.sales.toLocaleString()}</h3></div>
        <div className="bg-white p-4 rounded-xl shadow-sm border border-red-100"><p className="text-xs text-slate-500 uppercase">{t('total_expense')}</p><h3 className="text-2xl font-bold text-red-600">৳{stats.expenses.toLocaleString()}</h3></div>
        <div className="bg-white p-4 rounded-xl shadow-sm border border-blue-100"><p className="text-xs text-slate-500 uppercase">{t('net_profit')}</p><h3 className="text-2xl font-bold text-blue-600">৳{stats.profit.toLocaleString()}</h3></div>
        <div className="bg-white p-4 rounded-xl shadow-sm border border-amber-100"><p className="text-xs text-slate-500 uppercase">{t('pending_delivery')}</p><h3 className="text-2xl font-bold text-amber-600">{stats.pendingDelivery}</h3></div>
      </div>
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        {[
          { label: t('new_sale'), icon: ShoppingCart, color: 'bg-emerald-600', action: () => setTab('sales') },
          { label: t('courier'), icon: Truck, color: 'bg-indigo-600', action: () => setTab('delivery') },
          { label: t('bulk_sms'), icon: MessageSquare, color: 'bg-pink-600', action: () => setTab('marketing') },
          { label: t('expenses'), icon: Wallet, color: 'bg-red-500', action: () => setTab('expenses') },
        ].map((btn, idx) => (
          <button key={idx} onClick={btn.action} className={`${btn.color} text-white p-4 rounded-xl shadow-lg transition-all text-left flex flex-col justify-between h-24`}>
            <btn.icon className="w-6 h-6 opacity-80" />
            <div className="font-bold">{btn.label}</div>
          </button>
        ))}
      </div>
    </div>
  );
}

function ReturnExchangeView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [section, setSection] = useState('return'); // return, exchange
  const [activeSub, setActiveSub] = useState('create'); // create, history, products

  const renderContent = () => {
    if (section === 'return') {
      if (activeSub === 'create') return <ReturnCreate t={t} />;
      if (activeSub === 'history') return <ReturnHistory t={t} />;
      if (activeSub === 'products') return <ReturnProducts t={t} />;
    } else {
      if (activeSub === 'create') return <ExchangeCreate t={t} />;
      if (activeSub === 'history') return <ExchangeHistory t={t} />;
      if (activeSub === 'products') return <ExchangeProducts t={t} />;
    }
    return null;
  };

  return (
    <div className="flex flex-col lg:flex-row h-full gap-4 -m-4 lg:-m-8">
      {/* Sub Sidebar */}
      <div className="w-full lg:w-64 bg-[#3b0764] text-white flex-shrink-0 flex flex-col">
         <div className="p-6">
           <h3 className="font-bold text-lg mb-6 flex items-center gap-2"><ShoppingCart size={20}/> {t('return')} & {t('exchange')}</h3>
           
           <div className="space-y-6">
             {/* Return Section */}
             <div>
                <button onClick={() => setSection('return')} className="flex items-center justify-between w-full font-bold mb-2 text-purple-200 hover:text-white">
                   <div className="flex items-center gap-2"><RefreshCcw size={16}/> {t('return')}</div>
                   {section === 'return' ? <ChevronUp size={16}/> : <ChevronDown size={16}/>}
                </button>
                {section === 'return' && (
                  <div className="pl-6 space-y-1">
                    <button onClick={() => setActiveSub('create')} className={`block w-full text-left py-2 text-sm ${activeSub==='create' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('sales_return')}</button>
                    <button onClick={() => setActiveSub('history')} className={`block w-full text-left py-2 text-sm ${activeSub==='history' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('return_history')}</button>
                    <button onClick={() => setActiveSub('products')} className={`block w-full text-left py-2 text-sm ${activeSub==='products' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('return_products')}</button>
                  </div>
                )}
             </div>

             {/* Exchange Section */}
             <div>
                <button onClick={() => setSection('exchange')} className="flex items-center justify-between w-full font-bold mb-2 text-purple-200 hover:text-white">
                   <div className="flex items-center gap-2"><Repeat size={16}/> {t('exchange')}</div>
                   {section === 'exchange' ? <ChevronUp size={16}/> : <ChevronDown size={16}/>}
                </button>
                {section === 'exchange' && (
                  <div className="pl-6 space-y-1">
                    <button onClick={() => setActiveSub('create')} className={`block w-full text-left py-2 text-sm ${activeSub==='create' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('sales_exchange')}</button>
                    <button onClick={() => setActiveSub('history')} className={`block w-full text-left py-2 text-sm ${activeSub==='history' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('exchange_history')}</button>
                    <button onClick={() => setActiveSub('products')} className={`block w-full text-left py-2 text-sm ${activeSub==='products' ? 'text-white font-bold' : 'text-purple-300 hover:text-white'}`}>{t('exchange_products')}</button>
                  </div>
                )}
             </div>
           </div>
         </div>
      </div>

      {/* Main Content */}
      <div className="flex-1 p-6 overflow-y-auto bg-slate-50">
         {renderContent()}
      </div>
    </div>
  );
}

// Sub-components for Return & Exchange
function ReturnCreate({ t }) {
  const [deduction, setDeduction] = useState(0);
  const [paidAmount, setPaidAmount] = useState(0);

  return (
    <div className="space-y-4">
       <div className="bg-white p-3 rounded-full flex gap-4 w-fit px-6 shadow-sm">
         <button className="bg-black text-white px-4 py-1 rounded-full text-sm font-bold">Return Create</button>
         <button className="text-slate-500 px-4 py-1 text-sm font-medium hover:text-black">Return History</button>
         <button className="text-slate-500 px-4 py-1 text-sm font-medium hover:text-black">Return Products</button>
       </div>

       <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <div className="lg:col-span-2">
            <Card className="p-4 h-full">
              <h3 className="font-bold mb-4">Return Invoice</h3>
              <div className="relative mb-4">
                 <input className="w-full p-3 pl-10 border rounded focus:outline-none focus:border-orange-500" placeholder="Scan/Type Product ID or Name"/>
                 <ScanLine className="absolute left-3 top-3.5 text-orange-500" size={18}/>
              </div>
              <div className="h-64 bg-slate-50 rounded flex items-center justify-center text-slate-400">
                Cart is empty
              </div>
            </Card>
          </div>
          
          <div>
            <Card className="p-6 space-y-4">
              <h3 className="font-bold text-lg mb-2">{t('billing_summary')}</h3>
              <div className="flex gap-2 mb-2">
                 <label className="flex-1 flex items-center gap-2 p-2 border rounded cursor-pointer border-emerald-500 bg-emerald-50">
                    <input type="radio" name="rtype" defaultChecked className="text-emerald-500"/> <span className="text-sm font-bold">{t('retail_return')}</span>
                 </label>
                 <label className="flex-1 flex items-center gap-2 p-2 border rounded cursor-pointer">
                    <input type="radio" name="rtype"/> <span className="text-sm">{t('party_return')}</span>
                 </label>
              </div>
              <div><label className="text-xs font-bold text-slate-500">Client Name *</label><select className="w-full p-2 border rounded"><option>Select Client</option></select></div>
              <div><label className="text-xs font-bold text-slate-500">Phone Number *</label><input className="w-full p-2 border rounded bg-slate-50" placeholder="Type Phone number"/></div>
              <div><label className="text-xs font-bold text-slate-500">Address</label><input className="w-full p-2 border rounded" placeholder="Enter Address"/></div>
              
              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">Total Qty</label><input className="w-full p-2 border rounded bg-slate-50" value="0" readOnly/></div>
                 <div><label className="text-xs font-bold text-slate-500">Amount</label><input className="w-full p-2 border rounded bg-slate-50" value="0.00" readOnly/></div>
              </div>
              
              <div><label className="text-xs font-bold text-slate-500">VAT</label><input className="w-full p-2 border rounded bg-slate-50" value="0" readOnly/></div>
              
              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">{t('total_deduction')}</label><input type="number" className="w-full p-2 border rounded" value={deduction} onChange={(e) => setDeduction(e.target.value)}/></div>
                 <div><label className="text-xs font-bold text-slate-500">{t('total_returnable')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0.00" readOnly/></div>
              </div>

              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">Payment Method *</label><select className="w-full p-2 border rounded"><option>Payment</option></select></div>
                 <div className="flex gap-2 items-end">
                    <div className="flex-1"><label className="text-xs font-bold text-slate-500">Paid Amount *</label><input type="number" className="w-full p-2 border rounded bg-slate-50" value={paidAmount} onChange={(e) => setPaidAmount(e.target.value)}/></div>
                    <Trash2 size={18} className="mb-3 text-purple-400"/>
                 </div>
              </div>
              
              <Button className="w-full bg-emerald-600 hover:bg-emerald-700">{t('submit_return')}</Button>
            </Card>
          </div>
       </div>
    </div>
  );
}

function ReturnHistory({ t }) {
  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <input className="p-2 border rounded w-64" placeholder="Type here..." />
         <select className="p-2 border rounded w-40"><option>Select Outlet</option></select>
         <select className="p-2 border rounded w-32"><option>All</option></select>
         <div className="flex items-center gap-2 bg-white border rounded p-1"><input type="date" className="outline-none text-sm"/></div>
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Printer size={18}/></button>
         </div>
      </div>
      <table className="w-full text-left text-sm">
        <thead className="bg-slate-100 border-b font-bold text-slate-700">
           <tr><th className="p-3">SL.</th><th className="p-3">Date & Time</th><th className="p-3">Invoice No.</th><th className="p-3">Customer</th><th className="p-3">Phone</th><th className="p-3">Type</th><th className="p-3">Quantity</th><th className="p-3">Amount</th><th className="p-3">Action</th></tr>
        </thead>
        <tbody>
           <tr><td colSpan="9" className="p-8 text-center text-slate-400">No Data Available</td></tr>
        </tbody>
      </table>
    </Card>
  );
}

function ReturnProducts({ t }) {
  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <input className="p-2 border rounded w-64" placeholder="Type here..." />
         <select className="p-2 border rounded w-40"><option>Select Outlet</option></select>
         <select className="p-2 border rounded w-32"><option>All</option></select>
         <select className="p-2 border rounded w-40"><option>Category</option></select>
         <select className="p-2 border rounded w-40"><option>Brand</option></select>
         <div className="flex items-center gap-2 bg-white border rounded p-1"><input type="date" className="outline-none text-sm"/></div>
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Printer size={18}/></button>
         </div>
      </div>
      <table className="w-full text-left text-sm">
        <thead className="bg-slate-100 border-b font-bold text-slate-700">
           <tr><th className="p-3">SL.</th><th className="p-3">Category</th><th className="p-3">Brand</th><th className="p-3">Product</th><th className="p-3">Quantity</th><th className="p-3">Price</th></tr>
        </thead>
        <tbody>
           <tr><td colSpan="6" className="p-8 text-center text-slate-400">No Data Available</td></tr>
        </tbody>
      </table>
    </Card>
  );
}

function ExchangeCreate({ t }) {
  const [discount, setDiscount] = useState(0);
  const [paidAmount, setPaidAmount] = useState(0);

  return (
    <div className="space-y-4">
       <div className="bg-white p-3 rounded-full flex gap-4 w-fit px-6 shadow-sm">
         <button className="bg-black text-white px-4 py-1 rounded-full text-sm font-bold">Exchange Create</button>
         <button className="text-slate-500 px-4 py-1 text-sm font-medium hover:text-black">Exchange History</button>
         <button className="text-slate-500 px-4 py-1 text-sm font-medium hover:text-black">Exchange Products</button>
         <button className="text-black bg-emerald-900 text-white px-4 py-1 text-sm font-bold rounded-full">Return Products</button>
       </div>

       <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <div className="lg:col-span-2 space-y-4">
            <Card className="p-4">
              <h3 className="font-bold mb-2">Return Product</h3>
              <div className="relative">
                 <input className="w-full p-3 pl-10 border rounded focus:outline-none focus:border-orange-500" placeholder="Scan/Type Product ID or Name"/>
                 <ScanLine className="absolute left-3 top-3.5 text-orange-500" size={18}/>
              </div>
            </Card>
            <Card className="p-4">
              <h3 className="font-bold mb-2">Exchange Product</h3>
              <div className="relative">
                 <input className="w-full p-3 pl-10 border rounded focus:outline-none focus:border-orange-500" placeholder="Scan/Type Product ID or Name"/>
                 <ScanLine className="absolute left-3 top-3.5 text-orange-500" size={18}/>
              </div>
            </Card>
            <div className="h-40 bg-slate-100 rounded flex items-center justify-center text-slate-400">Cart Empty</div>
          </div>
          
          <div>
            <Card className="p-6 space-y-4">
              <h3 className="font-bold text-lg mb-2">Exchange Summary</h3>
              <div><label className="text-xs font-bold text-slate-500">Phone Number *</label><input className="w-full p-2 border rounded" placeholder="Type Phone number"/></div>
              <div><label className="text-xs font-bold text-slate-500">Customer Name *</label><input className="w-full p-2 border rounded" placeholder="Enter Customer Name"/></div>
              <div><label className="text-xs font-bold text-slate-500">Address</label><input className="w-full p-2 border rounded" placeholder="Enter Address"/></div>
              
              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">{t('return_qty')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0" readOnly/></div>
                 <div><label className="text-xs font-bold text-slate-500">{t('return_value')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0.00" readOnly/></div>
              </div>
              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">{t('exchange_qty')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0" readOnly/></div>
                 <div><label className="text-xs font-bold text-slate-500">{t('exchange_value')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0.00" readOnly/></div>
              </div>
              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">Total Discount</label><input type="number" className="w-full p-2 border rounded" value={discount} onChange={(e) => setDiscount(e.target.value)}/></div>
                 <div><label className="text-xs font-bold text-slate-500">{t('payable_amount')}</label><input className="w-full p-2 border rounded bg-slate-50" value="0.00" readOnly/></div>
              </div>

              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">Payment Method *</label><select className="w-full p-2 border rounded"><option>Payment</option></select></div>
                 <div className="flex gap-2 items-end">
                    <div className="flex-1"><label className="text-xs font-bold text-slate-500">Paid Amount *</label><input type="number" className="w-full p-2 border rounded bg-slate-50" value={paidAmount} onChange={(e) => setPaidAmount(e.target.value)}/></div>
                    <Trash2 size={18} className="mb-3 text-purple-400"/>
                 </div>
              </div>

              <div className="grid grid-cols-2 gap-4">
                 <div><label className="text-xs font-bold text-slate-500">Service Staff</label><select className="w-full p-2 border rounded"><option>Select Staff</option></select></div>
                 <div><label className="text-xs font-bold text-slate-500">Change Amount</label><input className="w-full p-2 border rounded bg-slate-50" value="0" readOnly/></div>
              </div>
              
              <Button className="w-full bg-emerald-700 hover:bg-emerald-800">{t('submit_exchange')}</Button>
            </Card>
          </div>
       </div>
    </div>
  );
}

function ExchangeHistory({ t }) {
  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <input className="p-2 border rounded w-64" placeholder="Type here..." />
         <select className="p-2 border rounded w-40"><option>Select Outlet</option></select>
         <div className="flex items-center gap-2 bg-white border rounded p-1"><input type="date" className="outline-none text-sm"/></div>
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Printer size={18}/></button>
         </div>
      </div>
      <table className="w-full text-left text-sm">
        <thead className="bg-slate-100 border-b font-bold text-slate-700">
           <tr><th className="p-3">SL.</th><th className="p-3">Date & Time</th><th className="p-3">Invoice No.</th><th className="p-3">Customer</th><th className="p-3">Phone</th><th className="p-3">Type</th><th className="p-3">Quantity</th><th className="p-3">Amount</th><th className="p-3">Action</th></tr>
        </thead>
        <tbody>
           <tr><td colSpan="9" className="p-8 text-center text-slate-400">No Data Available</td></tr>
        </tbody>
      </table>
    </Card>
  );
}

function ExchangeProducts({ t }) {
   // Same structure as Return Products
   return <ReturnProducts t={t} />; 
}

// 2. SALES VIEW (Refactored)
function SalesView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [subTab, setSubTab] = useState('create'); // create, history, product, customer

  const tabs = [
    { id: 'create', label: t('create_invoice'), icon: Plus },
    { id: 'history', label: t('sold_history'), icon: History },
    { id: 'product', label: t('sold_products'), icon: Package },
    { id: 'customer', label: t('customer_history'), icon: Users }
  ];

  return (
    <div className="h-full flex flex-col space-y-4">
      <div className="flex justify-between items-center">
        <h2 className="text-2xl font-bold">{t('sales')}</h2>
      </div>
      {/* Sales Sub Navigation */}
      <div className="flex gap-2 overflow-x-auto pb-2 border-b">
        {tabs.map(tab => (
           <button 
             key={tab.id}
             onClick={() => setSubTab(tab.id)}
             className={`flex items-center gap-2 px-4 py-2 rounded-lg font-medium transition-colors whitespace-nowrap ${subTab === tab.id ? 'bg-blue-100 text-blue-700' : 'text-slate-600 hover:bg-slate-50'}`}
           >
             <tab.icon size={16}/> {tab.label}
           </button>
        ))}
      </div>

      <div className="flex-1 overflow-y-auto">
        {subTab === 'create' && <CreateInvoice user={user} appId={appId} lang={lang} />}
        {subTab === 'history' && <SoldHistory user={user} appId={appId} lang={lang} />}
        {subTab === 'product' && <SoldProductHistory user={user} appId={appId} lang={lang} />}
        {subTab === 'customer' && <CustomerHistory user={user} appId={appId} lang={lang} />}
      </div>
    </div>
  );
}

// 2.1 Create Invoice Component
function CreateInvoice({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [saleType, setSaleType] = useState('retail'); // retail, wholesale
  const [custPhone, setCustPhone] = useState('');
  const [custName, setCustName] = useState('');
  const [custAddress, setCustAddress] = useState('');
  const [cart, setCart] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [products, setProducts] = useState([]);
  
  // Financials
  const [additionalExpense, setAdditionalExpense] = useState(0);
  const [vat, setVat] = useState(0);
  const [discount, setDiscount] = useState(0);
  const [paidAmount, setPaidAmount] = useState(0);
  const [paymentMethod, setPaymentMethod] = useState('Cash');
  const [remarks, setRemarks] = useState('');
  const [serviceStaff, setServiceStaff] = useState('');

  // Fetch Products & Parties to auto-fill
  useEffect(() => {
    if (!user) return;
    const unsubProd = onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), snap => setProducts(snap.docs.map(d => ({id: d.id, ...d.data()}))));
    return () => unsubProd();
  }, [user, appId]);

  // Search Party by phone
  useEffect(() => {
    if(custPhone.length > 3) {
       // Ideally query db, but for this demo filtering local is okay if dataset small, or strict query better
       // For demo, we rely on user input. We could add a query here to auto-fill name/address.
    }
  }, [custPhone]);

  const addToCart = (p) => {
    setCart(prev => {
      const exist = prev.find(item => item.id === p.id);
      if (exist) return prev.map(item => item.id === p.id ? {...item, qty: item.qty + 1} : item);
      return [...prev, {...p, qty: 1}];
    });
    setSearchTerm(''); // clear search to continue scanning/typing
  };

  const removeFromCart = (idx) => {
    const newCart = [...cart];
    newCart.splice(idx, 1);
    setCart(newCart);
  };

  const totalQty = cart.reduce((acc, item) => acc + item.qty, 0);
  const subtotal = cart.reduce((acc, item) => acc + (item.price * item.qty), 0);
  const totalPayable = subtotal + parseFloat(additionalExpense||0) + parseFloat(vat||0) - parseFloat(discount||0);
  const changeAmount = (parseFloat(paidAmount)||0) - totalPayable;

  const handleSale = async () => {
    if(!custPhone || !custName) { alert('Customer Name and Phone are required'); return; }
    if(cart.length === 0) { alert('Cart is empty'); return; }

    const batch = writeBatch(db);
    const txnRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'));
    
    // Create/Update Party (Simplified)
    // In a real app we'd check if party exists by phone, if not create. For now we assume new or update logic handled by backend or just logged in txn
    
    const saleData = {
      type: 'sale',
      saleType,
      partyPhone: custPhone,
      partyName: custName,
      partyAddress: custAddress,
      items: cart,
      totalQty,
      subtotal,
      additionalExpense: parseFloat(additionalExpense||0),
      vat: parseFloat(vat||0),
      discount: parseFloat(discount||0),
      amount: totalPayable,
      paidAmount: parseFloat(paidAmount||0),
      dueAmount: Math.max(0, totalPayable - (parseFloat(paidAmount)||0)),
      paymentMethod,
      changeAmount: Math.max(0, changeAmount),
      remarks,
      serviceStaff,
      date: serverTimestamp(),
      deliveryStatus: 'Delivered' // Default to delivered for instant sales
    };

    batch.set(txnRef, saleData);

    // Stock updates
    cart.forEach(item => {
       if(item.id) {
         batch.update(doc(db, 'artifacts', appId, 'public', 'data', 'products', item.id), { stock: increment(-item.qty) });
         const logRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'stock_logs'));
         batch.set(logRef, {
           productId: item.id,
           productName: item.name,
           qty: -item.qty,
           type: 'sale',
           refId: txnRef.id,
           date: serverTimestamp()
         });
       }
    });

    await batch.commit();
    alert('Sale Completed!');
    setCart([]); setCustPhone(''); setCustName(''); setCustAddress(''); setPaidAmount(0);
  };

  const filteredProducts = products.filter(p => p.name.toLowerCase().includes(searchTerm.toLowerCase()) || p.sku?.toLowerCase().includes(searchTerm.toLowerCase()));

  return (
    <div className="flex flex-col gap-6">
      <Card className="p-6">
        <h3 className="font-bold text-lg mb-4 border-b pb-2">Invoice Summary</h3>
        {/* Sale Type */}
        <div className="flex gap-4 mb-6">
          <label className={`flex items-center gap-2 p-3 border rounded-lg cursor-pointer flex-1 justify-center transition-colors ${saleType==='retail' ? 'bg-blue-50 border-blue-500 text-blue-700 font-bold' : 'hover:bg-slate-50'}`}>
            <input type="radio" name="saleType" checked={saleType==='retail'} onChange={()=>setSaleType('retail')} className="w-4 h-4"/> {t('retail_sale')}
          </label>
          <label className={`flex items-center gap-2 p-3 border rounded-lg cursor-pointer flex-1 justify-center transition-colors ${saleType==='wholesale' ? 'bg-blue-50 border-blue-500 text-blue-700 font-bold' : 'hover:bg-slate-50'}`}>
            <input type="radio" name="saleType" checked={saleType==='wholesale'} onChange={()=>setSaleType('wholesale')} className="w-4 h-4"/> {t('wholesale')}
          </label>
        </div>

        {/* Customer Details */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
           <div>
             <label className="block text-sm font-medium mb-1">{t('phone_number')} <span className="text-red-500">*</span></label>
             <input className="w-full p-2 border rounded" placeholder="Type Phone number" value={custPhone} onChange={e=>setCustPhone(e.target.value)}/>
           </div>
           <div>
             <label className="block text-sm font-medium mb-1">{t('customer_name')} <span className="text-red-500">*</span></label>
             <input className="w-full p-2 border rounded" placeholder="Enter Customer Name" value={custName} onChange={e=>setCustName(e.target.value)}/>
           </div>
           <div className="md:col-span-2">
             <label className="block text-sm font-medium mb-1">{t('address')}</label>
             <input className="w-full p-2 border rounded" placeholder="Enter Address" value={custAddress} onChange={e=>setCustAddress(e.target.value)}/>
           </div>
        </div>

        {/* Product Search & Cart */}
        <div className="mb-6 relative">
           <label className="block text-sm font-medium mb-1">Search Product</label>
           <div className="relative">
             <input className="w-full p-2 pl-10 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Scan SKU or Search Product..." value={searchTerm} onChange={e=>setSearchTerm(e.target.value)}/>
             <Search className="absolute left-3 top-2.5 text-slate-400" size={18}/>
           </div>
           {searchTerm && (
             <div className="absolute z-10 w-full bg-white border rounded-b shadow-lg max-h-60 overflow-y-auto">
                {filteredProducts.map(p => (
                  <div key={p.id} onClick={()=>addToCart(p)} className="p-2 hover:bg-blue-50 cursor-pointer border-b flex justify-between">
                    <span>{p.name} ({p.sku})</span>
                    <span className="font-bold">৳{p.price}</span>
                  </div>
                ))}
             </div>
           )}
        </div>

        {/* Cart Table */}
        {cart.length > 0 && (
          <div className="border rounded-lg overflow-hidden mb-6">
            <table className="w-full text-left text-sm">
              <thead className="bg-slate-50 border-b">
                <tr>
                  <th className="p-3">Product</th>
                  <th className="p-3 w-20">Price</th>
                  <th className="p-3 w-20">Qty</th>
                  <th className="p-3 w-24 text-right">Total</th>
                  <th className="p-3 w-10"></th>
                </tr>
              </thead>
              <tbody>
                {cart.map((item, idx) => (
                  <tr key={idx} className="border-b last:border-0">
                    <td className="p-3">{item.name}</td>
                    <td className="p-3">৳{item.price}</td>
                    <td className="p-3 font-bold">{item.qty}</td>
                    <td className="p-3 text-right font-bold">৳{item.price*item.qty}</td>
                    <td className="p-3"><button onClick={()=>removeFromCart(idx)} className="text-red-500 hover:bg-red-50 p-1 rounded"><Trash2 size={16}/></button></td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        )}

        {/* Financial Summary Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-x-8 gap-y-4 bg-slate-50 p-4 rounded-xl">
           <div><label className="block text-sm font-medium mb-1">{t('total_qty')}</label><input className="w-full p-2 border rounded bg-white" readOnly value={totalQty}/></div>
           <div><label className="block text-sm font-medium mb-1">{t('amount')}</label><input className="w-full p-2 border rounded bg-white" readOnly value={subtotal}/></div>
           
           <div><label className="block text-sm font-medium mb-1">{t('additional_expense')}</label><input type="number" className="w-full p-2 border rounded" value={additionalExpense} onChange={e=>setAdditionalExpense(e.target.value)}/></div>
           <div><label className="block text-sm font-medium mb-1">{t('vat')}</label><input type="number" className="w-full p-2 border rounded" value={vat} onChange={e=>setVat(e.target.value)}/></div>
           
           <div><label className="block text-sm font-medium mb-1">{t('total_discount')}</label><input type="number" className="w-full p-2 border rounded" value={discount} onChange={e=>setDiscount(e.target.value)}/></div>
           <div><label className="block text-sm font-medium mb-1">{t('total_payable')}</label><input className="w-full p-2 border rounded bg-blue-100 font-bold text-blue-800" readOnly value={totalPayable}/></div>
           
           <div>
             <label className="block text-sm font-medium mb-1">{t('payment_method')} <span className="text-red-500">*</span></label>
             <select className="w-full p-2 border rounded" value={paymentMethod} onChange={e=>setPaymentMethod(e.target.value)}>
               <option>Cash</option><option>Card</option><option>bKash</option><option>Nagad</option>
             </select>
           </div>
           <div className="flex gap-2">
             <div className="flex-1">
               <label className="block text-sm font-medium mb-1">{t('paid_amount')} <span className="text-red-500">*</span></label>
               <input type="number" className="w-full p-2 border rounded font-bold" value={paidAmount} onChange={e=>setPaidAmount(e.target.value)}/>
             </div>
             <button className="mt-6 text-red-400 hover:text-red-600"><Trash2 size={20}/></button>
           </div>

           <div><label className="block text-sm font-medium mb-1">{t('change_amount')}</label><input className="w-full p-2 border rounded bg-slate-200 text-slate-600" readOnly value={changeAmount}/></div>
           <div>
             <label className="block text-sm font-medium mb-1">{t('service_staff')}</label>
             <select className="w-full p-2 border rounded" value={serviceStaff} onChange={e=>setServiceStaff(e.target.value)}>
               <option value="">Select Staff</option>
               <option value="Rahim">Rahim</option>
               <option value="Karim">Karim</option>
             </select>
           </div>
           
           <div className="md:col-span-2">
             <label className="block text-sm font-medium mb-1">{t('remarks')}</label>
             <textarea className="w-full p-2 border rounded h-20 resize-none" placeholder="Add remarks here..." value={remarks} onChange={e=>setRemarks(e.target.value)}></textarea>
           </div>
        </div>

        <Button className="w-full mt-6 py-3 text-lg" onClick={handleSale}>{t('confirm_sale')}</Button>
      </Card>
    </div>
  );
}

// 2.2 Sold History Component
function SoldHistory({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [sales, setSales] = useState([]);
  
  useEffect(() => {
    if(!user) return;
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(q, snap => {
      const allData = snap.docs.map(d => ({id: d.id, ...d.data()}));
      // Filter and sort in JS
      const filtered = allData
        .filter(d => d.type === 'sale')
        .sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0));
      setSales(filtered);
    });
    return () => unsub();
  }, [user, appId]);

  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <input className="p-2 border rounded w-64" placeholder="Type here..." />
         <select className="p-2 border rounded w-40"><option>Select Outlet</option><option>Main</option></select>
         <select className="p-2 border rounded w-32"><option>All</option><option>Paid</option><option>Due</option></select>
         <input type="date" className="p-2 border rounded" />
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Printer size={18}/></button>
         </div>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full text-left text-sm whitespace-nowrap">
          <thead className="bg-slate-100 border-b text-slate-600">
            <tr>
              <th className="p-3 font-bold">{t('sl')}</th>
              <th className="p-3 font-bold">Date & Time</th>
              <th className="p-3 font-bold">{t('invoice_no')}</th>
              <th className="p-3 font-bold">Customer</th>
              <th className="p-3 font-bold">Phone</th>
              <th className="p-3 font-bold">{t('type')}</th>
              <th className="p-3 font-bold text-center">Quantity</th>
              <th className="p-3 font-bold text-right">Amount</th>
              <th className="p-3 font-bold text-right">Action</th>
            </tr>
          </thead>
          <tbody>
            {sales.map((sale, idx) => (
              <tr key={sale.id} className="border-b hover:bg-slate-50">
                <td className="p-3">{idx+1}</td>
                <td className="p-3">{sale.date?.toDate ? sale.date.toDate().toLocaleString() : 'N/A'}</td>
                <td className="p-3 font-mono text-xs">{sale.id.slice(0,8).toUpperCase()}</td>
                <td className="p-3">{sale.partyName}</td>
                <td className="p-3">{sale.partyPhone}</td>
                <td className="p-3"><span className="px-2 py-1 bg-blue-50 text-blue-700 rounded text-xs capitalize">{sale.saleType || 'Retail'}</span></td>
                <td className="p-3 text-center">{sale.totalQty}</td>
                <td className="p-3 text-right font-bold">৳{sale.amount}</td>
                <td className="p-3 text-right"><button className="text-blue-600 hover:bg-blue-50 p-1 rounded"><Printer size={16}/></button></td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </Card>
  );
}

// 2.3 Sold Product History
function SoldProductHistory({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [soldItems, setSoldItems] = useState([]);

  useEffect(() => {
    if(!user) return;
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(q, snap => {
       const items = [];
       const allData = snap.docs.map(d => ({id: d.id, ...d.data()}));
       const sales = allData
         .filter(d => d.type === 'sale')
         .sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0))
         .slice(0, 50); // Limit locally

       sales.forEach(data => {
          if(data.items && Array.isArray(data.items)) {
             data.items.forEach(item => {
                items.push({
                   txnId: data.id,
                   date: data.date,
                   category: item.category || 'General',
                   brand: item.brand || 'Generic',
                   name: item.name,
                   qty: item.qty,
                   price: item.price
                });
             });
          }
       });
       setSoldItems(items);
    });
    return () => unsub;
  }, [user, appId]);

  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <input className="p-2 border rounded w-64" placeholder="Type here..." />
         <select className="p-2 border rounded w-40"><option>Select Outlet</option><option>Main</option></select>
         <select className="p-2 border rounded w-32"><option>All</option></select>
         <select className="p-2 border rounded w-40"><option>Category</option></select>
         <select className="p-2 border rounded w-40"><option>Brand</option></select>
         <input type="date" className="p-2 border rounded" />
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Printer size={18}/></button>
         </div>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full text-left text-sm whitespace-nowrap">
          <thead className="bg-slate-100 border-b text-slate-600">
            <tr>
              <th className="p-3 font-bold">{t('sl')}</th>
              <th className="p-3 font-bold">{t('category')}</th>
              <th className="p-3 font-bold">{t('brand')}</th>
              <th className="p-3 font-bold">Product</th>
              <th className="p-3 font-bold text-center">{t('quantity')}</th>
              <th className="p-3 font-bold text-right">Price</th>
            </tr>
          </thead>
          <tbody>
            {soldItems.map((item, idx) => (
              <tr key={`${item.txnId}-${idx}`} className="border-b hover:bg-slate-50">
                <td className="p-3">{idx+1}</td>
                <td className="p-3">{item.category}</td>
                <td className="p-3">{item.brand}</td>
                <td className="p-3 font-medium">{item.name}</td>
                <td className="p-3 text-center">{item.qty}</td>
                <td className="p-3 text-right">৳{item.price}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </Card>
  );
}

// 2.4 Customer History Component
function CustomerHistory({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [history, setHistory] = useState([]);

  useEffect(() => {
    if(!user) return;
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(q, snap => {
       const items = [];
       const allData = snap.docs.map(d => ({id: d.id, ...d.data()}));
       const sales = allData
         .filter(d => d.type === 'sale')
         .sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0))
         .slice(0, 50);

       sales.forEach(data => {
          if(data.items && Array.isArray(data.items)) {
             data.items.forEach(item => {
                items.push({
                   txnId: data.id,
                   date: data.date,
                   orderNo: data.id.slice(0,8).toUpperCase(),
                   storeName: 'Main Store', // Hardcoded for demo
                   customerName: data.partyName,
                   customerPhone: data.partyPhone,
                   customerAddress: data.partyAddress || '-',
                   productName: item.name,
                   qty: item.qty,
                   serial: item.serial || '-'
                });
             });
          }
       });
       setHistory(items);
    });
    return () => unsub;
  }, [user, appId]);

  return (
    <Card>
      <div className="p-4 border-b flex flex-wrap gap-4 items-center bg-slate-50">
         <h3 className="font-bold mr-4">Customer History</h3>
         <input className="p-2 border rounded w-40" placeholder="Order Number" />
         <input className="p-2 border rounded w-40" placeholder="Customer Phone" />
         <select className="p-2 border rounded w-40"><option>Select Product</option></select>
         <select className="p-2 border rounded w-40"><option>Select Outlet</option></select>
         <input type="date" className="p-2 border rounded" />
         <div className="ml-auto flex gap-2">
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><FileText size={18}/></button>
            <button className="p-2 bg-white border rounded hover:bg-slate-50"><Download size={18}/></button>
         </div>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full text-left text-sm whitespace-nowrap">
          <thead className="bg-slate-100 border-b text-slate-600">
            <tr>
              <th className="p-3 font-bold">{t('sl')}</th>
              <th className="p-3 font-bold">{t('date')}</th>
              <th className="p-3 font-bold">Order No.</th>
              <th className="p-3 font-bold">{t('store_name')}</th>
              <th className="p-3 font-bold">{t('customer_name')}</th>
              <th className="p-3 font-bold">Customer Phone</th>
              <th className="p-3 font-bold">Customer Address</th>
              <th className="p-3 font-bold">Product Name</th>
              <th className="p-3 font-bold text-center">QTY</th>
              <th className="p-3 font-bold">Serial No.</th>
            </tr>
          </thead>
          <tbody>
            {history.map((row, idx) => (
              <tr key={`${row.txnId}-${idx}`} className="border-b hover:bg-slate-50">
                <td className="p-3">{idx+1}</td>
                <td className="p-3">{row.date?.toDate ? row.date.toDate().toLocaleDateString() : 'N/A'}</td>
                <td className="p-3 font-mono text-xs">{row.orderNo}</td>
                <td className="p-3">{row.storeName}</td>
                <td className="p-3">{row.customerName}</td>
                <td className="p-3">{row.customerPhone}</td>
                <td className="p-3 max-w-[150px] truncate" title={row.customerAddress}>{row.customerAddress}</td>
                <td className="p-3">{row.productName}</td>
                <td className="p-3 text-center">{row.qty}</td>
                <td className="p-3">{row.serial}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </Card>
  );
}

// 3. DELIVERY VIEW
function DeliveryView({ user, appId, lang }) {
    const t = (key) => TRANSLATIONS[lang][key] || key;
    const [deliveries, setDeliveries] = useState([]);

    useEffect(() => {
        if(!user) return;
        const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
        const unsub = onSnapshot(q, snap => {
            const allDocs = snap.docs.map(d => ({id: d.id, ...d.data()}));
            const filtered = allDocs.filter(d => d.deliveryMethod === 'Courier').sort((a, b) => (b.date?.toDate ? b.date.toDate().getTime() : 0) - (a.date?.toDate ? a.date.toDate().getTime() : 0));
            setDeliveries(filtered);
        });
        return () => unsub();
    }, [user, appId]);

    return (
        <div className="space-y-6">
            <h2 className="text-2xl font-bold text-slate-800">{t('delivery_tracking')}</h2>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                 {['Pending', 'Shipped', 'Delivered'].map(status => (<Card key={status} className="p-4 border-t-4 border-t-blue-500"><h3 className="font-bold text-slate-500 uppercase text-xs">{t(status.toLowerCase())}</h3><div className="text-3xl font-bold mt-1">{deliveries.filter(d => d.deliveryStatus === status).length}</div></Card>))}
            </div>
            <Card>
                <table className="w-full text-left text-sm"><thead className="bg-slate-50 border-b"><tr><th className="p-3">{t('order_id')}</th><th className="p-3">{t('customer')}</th><th className="p-3">{t('courier')}</th><th className="p-3">{t('tracking')}</th><th className="p-3">{t('status')}</th></tr></thead><tbody>{deliveries.map(d => (<tr key={d.id} className="border-b last:border-0"><td className="p-3 font-mono text-xs">{d.id.slice(0,8)}</td><td className="p-3">{d.partyName || 'Guest'}</td><td className="p-3">{d.courierName}</td><td className="p-3 font-mono text-xs bg-slate-100 rounded w-fit px-2">{d.trackingId}</td><td className="p-3"><span className={`px-2 py-1 rounded text-xs font-bold ${d.deliveryStatus==='Delivered'?'bg-emerald-100 text-emerald-700':'bg-amber-100 text-amber-700'}`}>{t(d.deliveryStatus?.toLowerCase() || 'pending')}</span></td></tr>))}</tbody></table>
            </Card>
        </div>
    );
}

// 4. MARKETING VIEW (Coupons & SMS)
function MarketingView({ user, appId, lang }) {
    const t = (key) => TRANSLATIONS[lang][key] || key;
    const [coupons, setCoupons] = useState([]);
    const [newCode, setNewCode] = useState('');
    const [discount, setDiscount] = useState('');
    const [smsMessage, setSmsMessage] = useState('');

    useEffect(() => {
        if(!user) return;
        const unsub = onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'coupons'), snap => setCoupons(snap.docs.map(d => ({id: d.id, ...d.data()}))));
        return () => unsub();
    }, [user, appId]);

    const addCoupon = async () => { if(!newCode || !discount) return; await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'coupons'), { code: newCode.toUpperCase(), discount: parseFloat(discount), active: true }); setNewCode(''); setDiscount(''); };
    const sendBulkSMS = () => { if(!smsMessage) return; alert(`Simulating SMS Blast: "${smsMessage}" sent!`); setSmsMessage(''); };

    return (
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <div className="space-y-6"><h2 className="text-xl font-bold">{t('coupon_management')}</h2><Card className="p-4 space-y-4"><div className="flex gap-2"><input className="flex-1 p-2 border rounded" placeholder="CODE (e.g. SALE10)" value={newCode} onChange={e => setNewCode(e.target.value)}/><input className="w-24 p-2 border rounded" placeholder="%" type="number" value={discount} onChange={e => setDiscount(e.target.value)}/><Button onClick={addCoupon}>{t('add')}</Button></div><div className="space-y-2">{coupons.map(c => (<div key={c.id} className="flex justify-between items-center p-3 border rounded bg-slate-50"><div><span className="font-bold text-blue-600">{c.code}</span> <span className="text-sm text-slate-500"> - {c.discount}% Off</span></div><button onClick={() => deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'coupons', c.id))} className="text-red-500"><Trash2 size={16}/></button></div>))}</div></Card></div>
            <div className="space-y-6"><h2 className="text-xl font-bold">{t('bulk_sms_marketing')}</h2><Card className="p-4 space-y-4"><div className="bg-blue-50 p-3 rounded text-sm text-blue-700 flex gap-2"><Smartphone size={16}/> {t('promo_sms')}</div><textarea className="w-full p-3 border rounded h-32" placeholder={t('type_message')} value={smsMessage} onChange={e => setSmsMessage(e.target.value)}></textarea><Button onClick={sendBulkSMS} icon={Send} className="w-full">{t('send_broadcast')}</Button></Card></div>
        </div>
    );
}

// 5. INVOICE / REPORTS VIEW
function ReportsView({ user, appId, lang }) {
    const t = (key) => TRANSLATIONS[lang][key] || key;
    const [sales, setSales] = useState([]);
    const [invoiceSale, setInvoiceSale] = useState(null);

    useEffect(() => {
        if (!user) return;
        const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
        const unsub = onSnapshot(q, snap => {
            const allDocs = snap.docs.map(d => ({id: d.id, ...d.data()}));
            const filtered = allDocs.filter(d => d.type === 'sale').sort((a, b) => (b.date?.toDate ? b.date.toDate().getTime() : 0) - (a.date?.toDate ? a.date.toDate().getTime() : 0));
            setSales(filtered);
        });
        return () => unsub();
    }, [user, appId]);

    return (
        <div className="space-y-6">
            <h2 className="text-2xl font-bold text-slate-800">{t('sales_report_invoice')}</h2>
            <Card>
                <table className="w-full text-left text-sm"><thead className="bg-slate-50 border-b"><tr><th className="p-3">{t('date')}</th><th className="p-3">{t('customer')}</th><th className="p-3">{t('amount')}</th><th className="p-3">{t('method')}</th><th className="p-3 text-right">{t('action')}</th></tr></thead><tbody>{sales.map(s => (<tr key={s.id} className="border-b hover:bg-slate-50"><td className="p-3">{s.date?.toDate ? s.date.toDate().toLocaleDateString() : 'N/A'}</td><td className="p-3">{s.partyName || 'Guest'}</td><td className="p-3 font-bold">৳{s.amount}</td><td className="p-3">{s.paymentMethod}</td><td className="p-3 text-right"><button onClick={() => setInvoiceSale(s)} className="bg-blue-100 text-blue-700 px-3 py-1 rounded text-xs font-bold hover:bg-blue-200 flex items-center gap-1 ml-auto"><Printer size={12}/> {t('invoice')}</button></td></tr>))}</tbody></table>
            </Card>

            {/* Invoice Modal */}
            {invoiceSale && (
                <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/70 p-4">
                    <div className="bg-white w-full max-w-2xl rounded-lg shadow-2xl overflow-hidden animate-in zoom-in-95 duration-200">
                        <div className="p-8 bg-white" id="invoice-area">
                            <div className="flex justify-between items-start border-b pb-6 mb-6"><div><h1 className="text-3xl font-bold text-slate-900">INVOICE</h1><p className="text-slate-500">#{invoiceSale.id.slice(-8).toUpperCase()}</p></div><div className="text-right"><h2 className="font-bold text-xl">SmartBiz POS</h2><p className="text-sm text-slate-500">Dhaka, Bangladesh</p></div></div>
                            <div className="flex justify-between mb-8 text-sm"><div><p className="font-bold text-slate-400 text-xs uppercase mb-1">{t('bill_to')}</p><p className="font-bold text-slate-800">{invoiceSale.partyName || 'Guest Customer'}</p>{invoiceSale.deliveryMethod === 'Courier' && (<p className="text-slate-500 mt-1">{t('via')} {invoiceSale.courierName} ({invoiceSale.trackingId})</p>)}</div><div className="text-right"><p className="font-bold text-slate-400 text-xs uppercase mb-1">{t('date')}</p><p className="font-bold text-slate-800">{invoiceSale.date?.toDate ? invoiceSale.date.toDate().toLocaleDateString() : 'N/A'}</p></div></div>
                            <table className="w-full text-sm mb-6"><thead className="bg-slate-50 text-slate-500"><tr><th className="p-3 text-left">{t('items')}</th><th className="p-3 text-center">{t('qty')}</th><th className="p-3 text-right">{t('total')}</th></tr></thead><tbody>{invoiceSale.items?.map((item, i) => (<tr key={i} className="border-b"><td className="p-3">{item.name}</td><td className="p-3 text-center">{item.qty}</td><td className="p-3 text-right">৳{item.price * item.qty}</td></tr>))}</tbody></table>
                            <div className="flex justify-end"><div className="w-48 space-y-2 text-sm"><div className="flex justify-between"><span>{t('subtotal')}:</span><span>৳{invoiceSale.subtotal}</span></div><div className="flex justify-between text-red-500"><span>{t('discount')}:</span><span>-৳{invoiceSale.discount}</span></div><div className="flex justify-between font-bold text-lg border-t pt-2"><span>{t('total')}:</span><span>৳{invoiceSale.amount}</span></div></div></div>
                        </div>
                        <div className="bg-slate-50 p-4 flex justify-end gap-2 border-t"><Button variant="secondary" onClick={() => setInvoiceSale(null)}>{t('close')}</Button><Button onClick={() => window.print()} icon={Printer}>{t('print_invoice')}</Button></div>
                    </div>
                </div>
            )}
        </div>
    );
}

// 6. EXPENSES VIEW
function ExpenseView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [amount, setAmount] = useState('');
  const [note, setNote] = useState('');
  const [category, setCategory] = useState('Rent');
  const [expenses, setExpenses] = useState([]);

  useEffect(() => {
    if (!user) return;
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(q, snap => {
        const allDocs = snap.docs.map(d => ({id: d.id, ...d.data()}));
        const filtered = allDocs.filter(d => d.type === 'expense').sort((a, b) => (b.date?.toDate ? b.date.toDate().getTime() : 0) - (a.date?.toDate ? a.date.toDate().getTime() : 0));
        setExpenses(filtered);
    });
    return () => unsub();
  }, [user, appId]);

  const addExpense = async () => { if (!amount) return; await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), { type: 'expense', amount: parseFloat(amount), paidAmount: parseFloat(amount), dueAmount: 0, date: serverTimestamp(), note, category }); setAmount(''); setNote(''); };

  return (
    <div className="max-w-4xl mx-auto space-y-6"><h2 className="text-2xl font-bold text-slate-800">{t('expenses')}</h2><Card className="p-6"><div className="grid grid-cols-1 md:grid-cols-4 gap-4 items-end"><div className="md:col-span-1"><label className="text-sm font-medium mb-1 block">{t('amount')}</label><input type="number" className="w-full p-2 border rounded" value={amount} onChange={e => setAmount(e.target.value)} placeholder="0.00"/></div><div className="md:col-span-1"><label className="text-sm font-medium mb-1 block">{t('category')}</label><select className="w-full p-2 border rounded" value={category} onChange={e => setCategory(e.target.value)}>{['Rent', 'Electricity', 'Salary', 'Tea', 'Transport', 'Other'].map(c => <option key={c} value={c}>{c}</option>)}</select></div><div className="md:col-span-1"><label className="text-sm font-medium mb-1 block">{t('note')}</label><input className="w-full p-2 border rounded" value={note} onChange={e => setNote(e.target.value)} placeholder="Note"/></div><div className="md:col-span-1"><Button onClick={addExpense} className="w-full">{t('add_expense')}</Button></div></div></Card><Card><table className="w-full text-left text-sm"><thead className="bg-slate-50 border-b"><tr><th className="p-4">{t('date')}</th><th className="p-4">{t('category')}</th><th className="p-4">{t('note')}</th><th className="p-4 text-right">{t('amount')}</th></tr></thead><tbody>{expenses.map(ex => (<tr key={ex.id} className="border-b"><td className="p-4">{ex.date?.toDate ? ex.date.toDate().toLocaleDateString() : 'N/A'}</td><td className="p-4"><span className="bg-slate-100 px-2 py-1 rounded text-xs">{ex.category}</span></td><td className="p-4 text-slate-600">{ex.note || '-'}</td><td className="p-4 text-right font-bold text-red-600">-৳{ex.amount}</td></tr>))}</tbody></table></Card></div>
  );
}

// 7. INVENTORY VIEW (New dedicated section)
function InventoryView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [subTab, setSubTab] = useState('list'); // list, report, detailed, ledger, transfer, serials
  const [products, setProducts] = useState([]);

  useEffect(() => { 
    if (!user) return; 
    return onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), snap => 
      setProducts(snap.docs.map(d => ({id: d.id, ...d.data()})))
    ); 
  }, [user, appId]);

  const tabs = [
    { id: 'list', label: t('product_list'), icon: Package },
    { id: 'report', label: t('stock_report'), icon: BarChart3 },
    { id: 'detailed', label: t('detailed_report'), icon: FileBarChart },
    { id: 'ledger', label: t('stock_ledger'), icon: History },
    { id: 'transfer', label: t('stock_transfer'), icon: ArrowRightLeft },
    { id: 'serials', label: t('serial_manage'), icon: Hash }
  ];

  return (
    <div className="space-y-6 h-full flex flex-col">
      <div className="flex justify-between items-center">
        <h2 className="text-2xl font-bold">{t('inv_dashboard')}</h2>
      </div>
      
      {/* Sub Navigation */}
      <div className="flex gap-2 overflow-x-auto pb-2 border-b">
        {tabs.map(tab => (
           <button 
             key={tab.id}
             onClick={() => setSubTab(tab.id)}
             className={`flex items-center gap-2 px-4 py-2 rounded-lg font-medium transition-colors whitespace-nowrap ${subTab === tab.id ? 'bg-blue-100 text-blue-700' : 'text-slate-600 hover:bg-slate-50'}`}
           >
             <tab.icon size={16}/> {tab.label}
           </button>
        ))}
      </div>

      <div className="flex-1 overflow-y-auto">
         {subTab === 'list' && <StockList products={products} user={user} appId={appId} lang={lang} />}
         {subTab === 'report' && <StockReportSummary products={products} lang={lang} />}
         {subTab === 'detailed' && <StockReportDetailed products={products} lang={lang} />}
         {subTab === 'ledger' && <StockLedger user={user} appId={appId} lang={lang} />}
         {subTab === 'transfer' && <StockTransfer products={products} user={user} appId={appId} lang={lang} />}
         {subTab === 'serials' && <SerialManager products={products} user={user} appId={appId} lang={lang} />}
      </div>
    </div>
  );
}

// 7.1 Stock List (Refactored old StockView)
function StockList({ products, user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [isEditing, setIsEditing] = useState(false);
  const [currentProduct, setCurrentProduct] = useState({});

  const handleSave = async () => { 
    const data = { 
      name: currentProduct.name, 
      sku: currentProduct.sku || 'SKU-'+Date.now(),
      category: currentProduct.category || 'General', 
      brand: currentProduct.brand || '',
      units: currentProduct.units || 'Pcs',
      size: currentProduct.size || '',
      variations: currentProduct.variations || '',
      color: currentProduct.color || '',
      warranty: currentProduct.warranty || '',
      mfgDate: currentProduct.mfgDate || '',
      expDate: currentProduct.expDate || '',
      cost: Number(currentProduct.cost || 0), 
      wholesalePrice: Number(currentProduct.wholesalePrice || 0),
      price: Number(currentProduct.price || 0), // MRP
      vat: Number(currentProduct.vat || 0),
      isVatApplicable: currentProduct.isVatApplicable || false,
      stock: Number(currentProduct.stock || 0), // Stock In
      alertQty: Number(currentProduct.alertQty || 0),
      // We don't save photo in this simplified version without storage
    }; 
    if (currentProduct.id) {
      await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', currentProduct.id), data); 
    } else { 
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), data); 
    }
    setIsEditing(false); 
  };

  const InputGroup = ({ label, required = false, children }) => (
    <div>
      <label className="block text-sm font-medium mb-1 text-slate-700">
        {label} {required && <span className="text-red-500">*</span>}
      </label>
      {children}
    </div>
  );

  return (
    <div className="space-y-4">
       <div className="flex justify-end">
          <Button onClick={() => { setCurrentProduct({units: 'Pcs'}); setIsEditing(true); }} icon={Plus}>{t('add_product')}</Button>
       </div>
       <Card>
        <table className="w-full text-left text-sm">
          <thead className="bg-slate-50 border-b"><tr><th className="p-3">{t('name')}</th><th className="p-3">{t('category')}</th><th className="p-3">{t('cost')}</th><th className="p-3">{t('price')}</th><th className="p-3">{t('stock')}</th><th className="p-3 text-right">{t('action')}</th></tr></thead>
          <tbody>
            {products.map(p => (
              <tr key={p.id} className={`border-b ${p.stock < (p.alertQty || 10) ? 'bg-red-50' : ''}`}>
                <td className="p-3">
                  <div className="font-medium">{p.name}</div>
                  <div className="text-xs text-slate-400">{p.sku}</div>
                  {p.stock < (p.alertQty || 10) && <div className="text-xs text-red-500 flex items-center gap-1 font-bold"><AlertTriangle size={10}/> {t('low_stock')}</div>}
                </td>
                <td className="p-3 text-slate-600">{p.category}</td>
                <td className="p-3">৳{p.cost}</td>
                <td className="p-3 text-blue-600 font-bold">৳{p.price}</td>
                <td className="p-3 font-bold">{p.stock}</td>
                <td className="p-3 text-right">
                  <button onClick={() => { setCurrentProduct(p); setIsEditing(true); }} className="text-slate-500 hover:text-blue-600 p-2 rounded hover:bg-slate-100"><Edit size={16}/></button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </Card>

      {/* Extended Add Product Modal */}
      {isEditing && (
        <Modal title={currentProduct.id ? 'Edit Product' : 'Add Product'} onClose={() => setIsEditing(false)} size="lg">
           <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
             {/* Row 1 */}
             <div className="md:col-span-2">
                <InputGroup label="Product Name" required>
                  <input className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter product name here..." value={currentProduct.name||''} onChange={e=>setCurrentProduct({...currentProduct,name:e.target.value})}/>
                </InputGroup>
             </div>
             <div>
                <InputGroup label={t('sku')}>
                   <div className="relative">
                      <input className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Scan/Type Here..." value={currentProduct.sku||''} onChange={e=>setCurrentProduct({...currentProduct,sku:e.target.value})}/>
                      <ScanLine size={16} className="absolute right-3 top-3 text-slate-400"/>
                   </div>
                </InputGroup>
             </div>

             {/* Row 2 */}
             <InputGroup label={t('category')} required>
               <div className="flex gap-1">
                 <select className="flex-1 p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.category} onChange={e=>setCurrentProduct({...currentProduct,category:e.target.value})}>
                   <option value="">Select Category</option>
                   <option value="Electronics">Electronics</option>
                   <option value="Fashion">Fashion</option>
                   <option value="Grocery">Grocery</option>
                   <option value="General">General</option>
                 </select>
                 <button className="p-2 border rounded bg-slate-50 hover:bg-slate-100 text-blue-600 font-bold">+</button>
               </div>
             </InputGroup>
             <InputGroup label={t('brand')}>
               <div className="flex gap-1">
                 <select className="flex-1 p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.brand} onChange={e=>setCurrentProduct({...currentProduct,brand:e.target.value})}>
                   <option value="">Select Brand</option>
                   <option value="Generic">Generic</option>
                   <option value="Samsung">Samsung</option>
                   <option value="Apple">Apple</option>
                   <option value="Nike">Nike</option>
                 </select>
                 <button className="p-2 border rounded bg-slate-50 hover:bg-slate-100 text-blue-600 font-bold">+</button>
               </div>
             </InputGroup>
             <InputGroup label={t('units')} required>
                 <select className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.units} onChange={e=>setCurrentProduct({...currentProduct,units:e.target.value})}>
                   <option value="Pcs">Pcs</option>
                   <option value="Kg">Kg</option>
                   <option value="Ltr">Ltr</option>
                   <option value="Box">Box</option>
                 </select>
             </InputGroup>

             {/* Row 3 */}
             <InputGroup label={t('size')}>
               <div className="flex gap-1">
                 <select className="flex-1 p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.size} onChange={e=>setCurrentProduct({...currentProduct,size:e.target.value})}>
                   <option value="">Select Size</option>
                   <option value="S">S</option>
                   <option value="M">M</option>
                   <option value="L">L</option>
                   <option value="XL">XL</option>
                 </select>
                 <button className="p-2 border rounded bg-slate-50 hover:bg-slate-100 text-blue-600 font-bold">+</button>
               </div>
             </InputGroup>
             <InputGroup label={t('variations')}>
               <div className="flex gap-1">
                 <select className="flex-1 p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.variations} onChange={e=>setCurrentProduct({...currentProduct,variations:e.target.value})}>
                   <option value="">Select Variations</option>
                   <option value="V1">V1</option>
                   <option value="V2">V2</option>
                 </select>
                 <button className="p-2 border rounded bg-slate-50 hover:bg-slate-100 text-blue-600 font-bold">+</button>
               </div>
             </InputGroup>
             <InputGroup label={t('color')}>
               <div className="flex gap-1">
                 <select className="flex-1 p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.color} onChange={e=>setCurrentProduct({...currentProduct,color:e.target.value})}>
                   <option value="">Select Color</option>
                   <option value="Red">Red</option>
                   <option value="Blue">Blue</option>
                   <option value="Black">Black</option>
                 </select>
                 <button className="p-2 border rounded bg-slate-50 hover:bg-slate-100 text-blue-600 font-bold">+</button>
               </div>
             </InputGroup>

             {/* Row 4 */}
             <InputGroup label={t('warranty')}>
               <div className="relative">
                 <select className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none appearance-none bg-white" value={currentProduct.warranty} onChange={e=>setCurrentProduct({...currentProduct,warranty:e.target.value})}>
                   <option value="">No warranty provided</option>
                   <option value="6 Months">6 Months</option>
                   <option value="1 Year">1 Year</option>
                   <option value="2 Years">2 Years</option>
                 </select>
                 <X size={14} className="absolute right-8 top-3 text-slate-400 cursor-pointer" onClick={()=>setCurrentProduct({...currentProduct, warranty: ''})} />
                 <ChevronRight size={14} className="absolute right-3 top-3 text-slate-400 rotate-90" />
               </div>
             </InputGroup>
             <InputGroup label={t('mfg_date')}>
               <div className="relative">
                 <input type="date" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.mfgDate||''} onChange={e=>setCurrentProduct({...currentProduct,mfgDate:e.target.value})} />
               </div>
             </InputGroup>
             <InputGroup label={t('exp_date')}>
               <div className="relative">
                 <input type="date" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" value={currentProduct.expDate||''} onChange={e=>setCurrentProduct({...currentProduct,expDate:e.target.value})} />
               </div>
             </InputGroup>

             {/* Row 5 - Pricing */}
             <InputGroup label={t('cost')} required>
               <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter Costing Price" value={currentProduct.cost||''} onChange={e=>setCurrentProduct({...currentProduct,cost:e.target.value})}/>
             </InputGroup>
             <InputGroup label={t('wholesale_price')} required>
               <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter Wholesale Price" value={currentProduct.wholesalePrice||''} onChange={e=>setCurrentProduct({...currentProduct,wholesalePrice:e.target.value})}/>
             </InputGroup>
             <InputGroup label={t('price')} required>
               <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter MRP" value={currentProduct.price||''} onChange={e=>setCurrentProduct({...currentProduct,price:e.target.value})}/>
             </InputGroup>

             {/* VAT */}
             <div className="flex gap-4 items-end">
                <div className="flex-1">
                  <InputGroup label={t('vat')}>
                    <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter VAT" value={currentProduct.vat||''} onChange={e=>setCurrentProduct({...currentProduct,vat:e.target.value})}/>
                  </InputGroup>
                </div>
                <div className="pb-3 flex items-center gap-2">
                   <input type="checkbox" id="isVat" className="w-4 h-4" checked={currentProduct.isVatApplicable||false} onChange={e=>setCurrentProduct({...currentProduct,isVatApplicable:e.target.checked})}/>
                   <label htmlFor="isVat" className="text-sm font-medium">{t('is_vat')}</label>
                </div>
             </div>
             
             {/* Row 6 */}
             <InputGroup label={t('alert_qty')}>
                <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="Enter Alert Qty" value={currentProduct.alertQty||''} onChange={e=>setCurrentProduct({...currentProduct,alertQty:e.target.value})}/>
             </InputGroup>
             <div className="relative border-2 border-dashed border-slate-300 rounded-lg p-4 flex flex-col items-center justify-center text-slate-400 bg-slate-50 hover:bg-slate-100 transition-colors cursor-pointer group">
                 <span className="text-xs absolute top-2 left-2 font-bold text-slate-500">{t('upload_photo')}</span>
                 <Upload size={24} className="mb-2 group-hover:text-blue-500"/>
                 <span className="text-xs text-center">Drag file here or Browse</span>
             </div>
             <InputGroup label={t('stock_in')}>
                <input type="number" className="w-full p-2 border rounded focus:ring-2 focus:ring-blue-500 outline-none" placeholder="0" value={currentProduct.stock||''} onChange={e=>setCurrentProduct({...currentProduct,stock:e.target.value})}/>
             </InputGroup>
           </div>
           
           <div className="flex justify-end gap-3 mt-6 pt-4 border-t">
              <Button variant="secondary" onClick={()=>setIsEditing(false)}>{t('close')}</Button>
              <Button onClick={handleSave}>{t('add')}</Button>
           </div>
        </Modal>
      )}
    </div>
  );
}

// 7.2 Stock Report Summary
function StockReportSummary({ products, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const totalItems = products.reduce((sum, p) => sum + (Number(p.stock)||0), 0);
  const totalCost = products.reduce((sum, p) => sum + ((Number(p.cost)||0) * (Number(p.stock)||0)), 0);
  const totalPrice = products.reduce((sum, p) => sum + ((Number(p.price)||0) * (Number(p.stock)||0)), 0);
  
  return (
    <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
       <Card className="p-6 bg-blue-50 border-blue-100"><h3 className="text-sm font-bold text-blue-700 uppercase mb-1">{t('total_items')}</h3><div className="text-3xl font-bold text-slate-800">{totalItems}</div></Card>
       <Card className="p-6 bg-emerald-50 border-emerald-100"><h3 className="text-sm font-bold text-emerald-700 uppercase mb-1">{t('stock_value_cost')}</h3><div className="text-3xl font-bold text-slate-800">৳{totalCost.toLocaleString()}</div></Card>
       <Card className="p-6 bg-purple-50 border-purple-100"><h3 className="text-sm font-bold text-purple-700 uppercase mb-1">{t('stock_value_sales')}</h3><div className="text-3xl font-bold text-slate-800">৳{totalPrice.toLocaleString()}</div></Card>
       <Card className="p-6 bg-amber-50 border-amber-100"><h3 className="text-sm font-bold text-amber-700 uppercase mb-1">{t('est_profit')}</h3><div className="text-3xl font-bold text-slate-800">৳{(totalPrice - totalCost).toLocaleString()}</div></Card>
    </div>
  );
}

// 7.3 Detailed Report
function StockReportDetailed({ products, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  return (
    <Card>
      <div className="p-4 border-b flex justify-between items-center bg-slate-50">
         <h3 className="font-bold">{t('detailed_report')}</h3>
         <Button icon={Printer} variant="secondary" onClick={() => window.print()} className="print:hidden">Print</Button>
      </div>
      <table className="w-full text-left text-sm">
        <thead className="bg-slate-50 border-b">
          <tr>
            <th className="p-3">{t('name')}</th>
            <th className="p-3 text-right">{t('stock')}</th>
            <th className="p-3 text-right">{t('cost')}</th>
            <th className="p-3 text-right">{t('price')}</th>
            <th className="p-3 text-right">Value (Cost)</th>
            <th className="p-3 text-right">Value (Sales)</th>
          </tr>
        </thead>
        <tbody>
          {products.map(p => {
             const qty = Number(p.stock)||0;
             const cost = Number(p.cost)||0;
             const price = Number(p.price)||0;
             return (
               <tr key={p.id} className="border-b">
                 <td className="p-3 font-medium">{p.name}</td>
                 <td className="p-3 text-right">{qty}</td>
                 <td className="p-3 text-right">৳{cost}</td>
                 <td className="p-3 text-right">৳{price}</td>
                 <td className="p-3 text-right font-bold text-slate-600">৳{(qty * cost).toLocaleString()}</td>
                 <td className="p-3 text-right font-bold text-emerald-600">৳{(qty * price).toLocaleString()}</td>
               </tr>
             )
          })}
        </tbody>
      </table>
    </Card>
  );
}

// 7.4 Stock Ledger
function StockLedger({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [logs, setLogs] = useState([]);
  useEffect(() => {
    if(!user) return;
    // Removed orderBy to prevent index errors
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'stock_logs');
    return onSnapshot(q, snap => {
      const allData = snap.docs.map(d => ({id: d.id, ...d.data()}));
      // Sort in JS
      allData.sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0));
      setLogs(allData.slice(0, 50));
    });
  }, [user, appId]);

  return (
    <Card>
       <div className="p-4 border-b bg-slate-50"><h3 className="font-bold">{t('stock_ledger')} (Last 50)</h3></div>
       <table className="w-full text-left text-sm">
          <thead className="bg-slate-50 border-b"><tr><th className="p-3">{t('date')}</th><th className="p-3">{t('product')}</th><th className="p-3">{t('type')}</th><th className="p-3 text-right">{t('qty')}</th></tr></thead>
          <tbody>
            {logs.map(l => (
              <tr key={l.id} className="border-b">
                <td className="p-3 text-slate-500">{l.date?.toDate ? l.date.toDate().toLocaleString() : 'N/A'}</td>
                <td className="p-3 font-medium">{l.productName}</td>
                <td className="p-3"><span className="bg-slate-100 px-2 py-1 rounded text-xs uppercase">{l.type}</span></td>
                <td className={`p-3 text-right font-bold ${l.qty > 0 ? 'text-emerald-600' : 'text-red-600'}`}>{l.qty > 0 ? '+' : ''}{l.qty}</td>
              </tr>
            ))}
          </tbody>
       </table>
    </Card>
  );
}

// 7.5 Stock Transfer
function StockTransfer({ products, user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [selProd, setSelProd] = useState('');
  const [qty, setQty] = useState('');
  const [toLoc, setToLoc] = useState('');

  const handleTransfer = async () => {
    if (!selProd || !qty || !toLoc) return;
    const prod = products.find(p => p.id === selProd);
    if (!prod) return;
    const q = parseInt(qty);
    
    const batch = writeBatch(db);
    batch.update(doc(db, 'artifacts', appId, 'public', 'data', 'products', selProd), { stock: increment(-q) });
    
    const logRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'stock_logs'));
    batch.set(logRef, {
      productId: selProd,
      productName: prod.name,
      qty: -q,
      type: 'transfer_out',
      date: serverTimestamp(),
      note: `Transferred to ${toLoc}`
    });
    
    await batch.commit();
    alert('Stock Transferred Successfully');
    setQty(''); setToLoc('');
  };

  return (
    <div className="max-w-xl mx-auto">
       <Card className="p-6 space-y-4">
          <h3 className="font-bold text-lg mb-2 flex items-center gap-2"><ArrowRightLeft size={20}/> {t('stock_transfer')}</h3>
          <div><label className="block text-sm font-medium mb-1">{t('product')}</label><select className="w-full p-2 border rounded" value={selProd} onChange={e=>setSelProd(e.target.value)}><option value="">Select</option>{products.map(p=><option key={p.id} value={p.id}>{p.name} ({p.stock})</option>)}</select></div>
          <div className="flex gap-4">
             <div className="flex-1"><label className="block text-sm font-medium mb-1">{t('qty')}</label><input type="number" className="w-full p-2 border rounded" value={qty} onChange={e=>setQty(e.target.value)}/></div>
             <div className="flex-1"><label className="block text-sm font-medium mb-1">{t('to')} (Location)</label><input className="w-full p-2 border rounded" placeholder="e.g. Warehouse" value={toLoc} onChange={e=>setToLoc(e.target.value)}/></div>
          </div>
          <Button className="w-full" onClick={handleTransfer}>{t('transfer')}</Button>
       </Card>
    </div>
  );
}

// 7.6 Serial Manager
function SerialManager({ products, user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [selProd, setSelProd] = useState('');
  const [newSerial, setNewSerial] = useState('');
  const [serials, setSerials] = useState([]);

  useEffect(() => {
    if (!user || !selProd) { setSerials([]); return; }
    const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'serial_numbers'), where('productId', '==', selProd));
    return onSnapshot(q, snap => setSerials(snap.docs.map(d => ({id: d.id, ...d.data()}))));
  }, [user, appId, selProd]);

  const addSerial = async () => {
    if (!selProd || !newSerial) return;
    await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'serial_numbers'), {
      productId: selProd,
      serial: newSerial,
      status: 'available', // available, sold
      addedAt: serverTimestamp()
    });
    setNewSerial('');
  };

  const deleteSerial = async (id) => {
    await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'serial_numbers', id));
  };

  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-6 h-[calc(100vh-200px)]">
       <div className="md:col-span-1 border-r pr-4 overflow-y-auto">
          <h3 className="font-bold mb-3">{t('product_list')}</h3>
          {products.map(p => (
            <button key={p.id} onClick={()=>setSelProd(p.id)} className={`w-full text-left p-3 rounded mb-2 border ${selProd===p.id ? 'bg-blue-50 border-blue-300 ring-1 ring-blue-300' : 'bg-white hover:bg-slate-50'}`}>
              <div className="font-medium text-sm">{p.name}</div>
            </button>
          ))}
       </div>
       <div className="md:col-span-2 flex flex-col">
          {selProd ? (
             <>
               <Card className="p-4 mb-4 flex gap-2">
                 <input className="flex-1 p-2 border rounded" placeholder="Enter Serial Number" value={newSerial} onChange={e=>setNewSerial(e.target.value)}/>
                 <Button onClick={addSerial}>{t('add_serial')}</Button>
               </Card>
               <Card className="flex-1 overflow-y-auto">
                 <table className="w-full text-left text-sm">
                   <thead className="bg-slate-50 border-b"><tr><th className="p-3">{t('serial_no')}</th><th className="p-3">{t('status')}</th><th className="p-3 text-right">{t('action')}</th></tr></thead>
                   <tbody>
                     {serials.map(s => (
                       <tr key={s.id} className="border-b">
                         <td className="p-3 font-mono text-slate-700">{s.serial}</td>
                         <td className="p-3"><span className={`px-2 py-1 rounded text-xs ${s.status==='available'?'bg-emerald-100 text-emerald-700':'bg-slate-100 text-slate-500'}`}>{t(s.status)}</span></td>
                         <td className="p-3 text-right"><button onClick={()=>deleteSerial(s.id)} className="text-red-500 hover:bg-red-50 p-1 rounded"><Trash2 size={16}/></button></td>
                       </tr>
                     ))}
                     {serials.length === 0 && <tr><td colSpan="3" className="p-4 text-center text-slate-400">No serials found</td></tr>}
                   </tbody>
                 </table>
               </Card>
             </>
          ) : (
            <div className="flex-1 flex items-center justify-center text-slate-400">Select a product to manage serials</div>
          )}
       </div>
    </div>
  );
}

function PartiesView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [parties, setParties] = useState([]);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [newParty, setNewParty] = useState({ type: 'customer' });
  const [historyParty, setHistoryParty] = useState(null);
  const [partyHistory, setPartyHistory] = useState([]);
  const [partyStats, setPartyStats] = useState({ totalSpent: 0, totalDue: 0 });

  useEffect(() => { if (!user) return; return onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'parties'), snap => setParties(snap.docs.map(d => ({id: d.id, ...d.data()})))); }, [user, appId]);
  
  useEffect(() => {
    if(!historyParty) { setPartyHistory([]); return; }
    // Fetch all transactions and filter locally
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    const unsub = onSnapshot(q, snap => {
       const allDocs = snap.docs.map(d => ({id: d.id, ...d.data()}));
       const docs = allDocs
         .filter(d => d.partyId === historyParty.id)
         .sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0))
         .slice(0, 20);

       setPartyHistory(docs);
       const totalSpent = docs.reduce((sum, t) => sum + (t.amount || 0), 0);
       setPartyStats({ totalSpent, totalDue: historyParty.balance || 0 });
    });
    return () => unsub();
  }, [historyParty, appId]);

  const handleSave = async () => { await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'parties'), { ...newParty, balance: 0 }); setIsModalOpen(false); };
  
  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center"><h2 className="text-2xl font-bold">{t('party_list')}</h2><Button onClick={() => setIsModalOpen(true)} icon={Plus}>{t('add_party')}</Button></div>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <Card className="p-4"><h3 className="font-bold text-blue-700 mb-2">{t('customer')}</h3>{parties.filter(p=>p.type==='customer').map(p=><div key={p.id} className="flex justify-between items-center p-2 border-b last:border-0"><div><div className="font-medium">{p.name}</div><button onClick={()=>setHistoryParty(p)} className="text-xs text-blue-500 hover:underline flex items-center gap-1"><Clock size={10}/> {t('view_history')}</button></div><span className={p.balance>0?'text-red-500 font-bold':'text-emerald-500'}>{t('due')}: ৳{p.balance}</span></div>)}</Card>
        <Card className="p-4"><h3 className="font-bold text-amber-700 mb-2">{t('suppliers')}</h3>{parties.filter(p=>p.type==='supplier').map(p=><div key={p.id} className="flex justify-between items-center p-2 border-b last:border-0"><div><div className="font-medium">{p.name}</div><button onClick={()=>setHistoryParty(p)} className="text-xs text-blue-500 hover:underline flex items-center gap-1"><Clock size={10}/> {t('view_history')}</button></div><span className={p.balance>0?'text-emerald-500':'text-red-500 font-bold'}>Bal: ৳{p.balance}</span></div>)}</Card>
      </div>
      
      {/* Add Party Modal */}
      {isModalOpen && (
        <Modal title={t('add_party')} onClose={() => setIsModalOpen(false)}>
           <div className="space-y-3">
             <select className="w-full p-2 border rounded" value={newParty.type} onChange={e=>setNewParty({...newParty,type:e.target.value})}><option value="customer">{t('customer')}</option><option value="supplier">{t('supplier')}</option></select>
             <input className="w-full p-2 border rounded" placeholder={t('name')} value={newParty.name||''} onChange={e=>setNewParty({...newParty,name:e.target.value})}/>
             <input className="w-full p-2 border rounded" placeholder="Phone" value={newParty.phone||''} onChange={e=>setNewParty({...newParty,phone:e.target.value})}/>
           </div>
           <div className="mt-4 flex justify-end gap-2"><Button variant="secondary" onClick={()=>setIsModalOpen(false)}>{t('close')}</Button><Button onClick={handleSave}>{t('add')}</Button></div>
        </Modal>
      )}

      {/* History Modal */}
      {historyParty && (
        <Modal title={`${historyParty.name} - ${t('history')}`} onClose={() => setHistoryParty(null)}>
           <div className="grid grid-cols-2 gap-4 mb-4">
             <div className="bg-blue-50 p-3 rounded text-center"><div className="text-xs text-blue-500 uppercase">{t('total_spent')}</div><div className="font-bold text-xl">৳{partyStats.totalSpent}</div></div>
             <div className="bg-red-50 p-3 rounded text-center"><div className="text-xs text-red-500 uppercase">{t('due')}</div><div className="font-bold text-xl">৳{partyStats.totalDue}</div></div>
           </div>
           <div className="space-y-2">
             <h4 className="font-bold text-sm text-slate-500 uppercase">{t('purchase_history')}</h4>
             {partyHistory.length === 0 ? <p className="text-slate-400 text-sm">No transactions found.</p> :
               partyHistory.map(txn => (
                 <div key={txn.id} className="border p-3 rounded hover:bg-slate-50">
                    <div className="flex justify-between mb-1">
                      <span className="font-bold text-slate-800">৳{txn.amount}</span>
                      <span className="text-xs text-slate-500">{txn.date?.toDate ? txn.date.toDate().toLocaleDateString() : 'N/A'}</span>
                    </div>
                    <div className="flex justify-between text-xs">
                      <span className="bg-slate-200 px-1 rounded">{txn.type}</span>
                      <span className={txn.dueAmount > 0 ? 'text-red-500' : 'text-emerald-500'}>{txn.dueAmount > 0 ? `Due: ৳${txn.dueAmount}` : 'Paid'}</span>
                    </div>
                 </div>
               ))
             }
           </div>
        </Modal>
      )}
    </div>
  );
}

function PurchaseView({ user, appId, lang }) {
    const t = (key) => TRANSLATIONS[lang][key] || key;
    const [suppliers, setSuppliers] = useState([]);
    const [products, setProducts] = useState([]);
    const [selSup, setSelSup] = useState('');
    const [selProd, setSelProd] = useState('');
    const [qty, setQty] = useState('');
    const [cost, setCost] = useState('');
    useEffect(() => { if(!user) return; onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'parties'), where('type', '==', 'supplier')), s=>setSuppliers(s.docs.map(d=>({id:d.id,...d.data()})))); onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), s=>setProducts(s.docs.map(d=>({id:d.id,...d.data()})))); }, [user, appId]);
    
    const handlePurchase = async () => { 
      if (!selSup || !selProd || !qty || !cost) return; 
      const total = parseFloat(cost) * parseInt(qty); 
      const batch = writeBatch(db); 
      
      const txnRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'));
      batch.set(txnRef, { type: 'purchase', amount: total, paidAmount: total, dueAmount: 0, date: serverTimestamp(), partyId: selSup }); 
      
      // Update Stock
      batch.update(doc(db, 'artifacts', appId, 'public', 'data', 'products', selProd), { stock: increment(parseInt(qty)), cost: parseFloat(cost) }); 
      
      // Inventory Management: Log Stock Increase
      const logRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'stock_logs'));
      const prodName = products.find(p => p.id === selProd)?.name || 'Unknown';
      batch.set(logRef, {
        productId: selProd,
        productName: prodName,
        qty: parseInt(qty), // Positive for purchase
        type: 'purchase',
        refId: txnRef.id,
        date: serverTimestamp()
      });

      await batch.commit(); 
      alert('Purchase Recorded!'); 
      setQty(''); setCost('');
    };
    
    return (
        <div className="max-w-xl mx-auto space-y-6"><h2 className="text-2xl font-bold">{t('purchase_stock')}</h2><Card className="p-6 space-y-4"><div><label className="block text-sm font-medium mb-1">{t('supplier')}</label><select className="w-full p-2 border rounded" value={selSup} onChange={e => setSelSup(e.target.value)}><option value="">Select</option>{suppliers.map(s => <option key={s.id} value={s.id}>{s.name}</option>)}</select></div><div><label className="block text-sm font-medium mb-1">{t('product')}</label><select className="w-full p-2 border rounded" value={selProd} onChange={e => setSelProd(e.target.value)}><option value="">Select</option>{products.map(p => <option key={p.id} value={p.id}>{p.name}</option>)}</select></div><div className="flex gap-4"><input type="number" className="flex-1 p-2 border rounded" placeholder={t('qty')} value={qty} onChange={e => setQty(e.target.value)} /><input type="number" className="flex-1 p-2 border rounded" placeholder={t('unit_cost')} value={cost} onChange={e => setCost(e.target.value)} /></div><Button onClick={handlePurchase} className="w-full">{t('confirm_purchase')}</Button></Card></div>
    );
}

function SettingsView({ user, appId, lang }) {
    const t = (key) => TRANSLATIONS[lang][key] || key;
    return <div className="max-w-2xl"><h2 className="text-2xl font-bold mb-6">{t('settings')}</h2><Card className="p-6"><h3 className="font-bold text-lg mb-4">{t('shop_config')}</h3><div className="space-y-4"><div><label className="block text-sm font-medium">{t('shop_name')}</label><input className="w-full p-2 border rounded" defaultValue="My Smart Shop"/></div><div><label className="block text-sm font-medium">{t('currency')}</label><input className="w-full p-2 border rounded" defaultValue="BDT (৳)" disabled/></div><Button>{t('save_changes')}</Button></div></Card></div>;
}

// 8. FAQ VIEW
function FAQView({ lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [openIndex, setOpenIndex] = useState(null);

  const faqs = [
    { question: t('q1'), answer: "To reset your password, go to settings and click on 'Change Password'. Follow the instructions sent to your email." },
    { question: t('q2'), answer: "Yes, we offer a 14-day free trial for all new users to explore our premium features." },
    { question: t('q3'), answer: "Absolutely! You can upgrade or downgrade your subscription plan at any time from the billing settings." },
    { question: t('q4'), answer: "Yes, we use industry-standard encryption to ensure your data is always safe and secure." },
    { question: t('q5'), answer: "We provide 24/7 customer support via email and live chat to assist you with any issues." },
  ];

  return (
    <div className="max-w-3xl mx-auto py-8 px-4">
      <div className="text-center mb-10">
        <h2 className="text-3xl font-bold text-slate-900 mb-2">{t('faq_title')}</h2>
        <p className="text-slate-500">{t('faq_subtitle')}</p>
      </div>

      <div className="space-y-4">
        {faqs.map((faq, idx) => (
          <div 
            key={idx} 
            className="bg-white rounded-xl shadow-sm border border-slate-100 overflow-hidden transition-all duration-200"
          >
            <button 
              onClick={() => setOpenIndex(openIndex === idx ? null : idx)}
              className="w-full p-5 flex items-center justify-between text-left hover:bg-slate-50 transition-colors"
            >
              <div className="flex items-center gap-4">
                <div className="w-8 h-8 rounded-full bg-orange-500 text-white flex items-center justify-center font-bold text-sm flex-shrink-0">
                  {idx + 1}
                </div>
                <span className="font-semibold text-slate-800 text-lg">{faq.question}</span>
              </div>
              {openIndex === idx ? <ChevronUp className="text-slate-400"/> : <ChevronDown className="text-slate-400"/>}
            </button>
            
            {openIndex === idx && (
              <div className="px-5 pb-5 pl-[4.5rem] text-slate-600 leading-relaxed animate-in slide-in-from-top-2">
                {faq.answer}
              </div>
            )}
          </div>
        ))}
      </div>
    </div>
  );
}

// 9. HELP & SUPPORT VIEW
function SupportView({ user, appId, lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [view, setView] = useState('list'); // 'list' | 'create'
  const [tickets, setTickets] = useState([]);
  const [newTicket, setNewTicket] = useState({ category: '', priority: '', subject: '', message: '' });

  useEffect(() => {
    if (!user) return;
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'tickets');
    const unsub = onSnapshot(q, snap => {
      const allDocs = snap.docs.map(d => ({id: d.id, ...d.data()}));
      // Sort client-side
      allDocs.sort((a, b) => (b.date?.seconds || 0) - (a.date?.seconds || 0));
      setTickets(allDocs);
    });
    return () => unsub();
  }, [user, appId]);

  const handleSubmit = async () => {
    if (!newTicket.category || !newTicket.subject || !newTicket.message) {
      alert("Please fill in required fields");
      return;
    }
    
    await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'tickets'), {
      ...newTicket,
      status: 'Open',
      date: serverTimestamp(),
      ticketId: `TKT-${Math.floor(Math.random() * 100000)}`
    });
    
    setNewTicket({ category: '', priority: '', subject: '', message: '' });
    setView('list');
    alert("Ticket Submitted Successfully!");
  };

  return (
    <div className="max-w-5xl mx-auto space-y-6">
      {/* Header Banner */}
      <div className="bg-purple-800 rounded-xl p-6 text-white flex justify-between items-center shadow-lg">
        <div>
          <h2 className="text-2xl font-bold mb-1">{t('ticket_list')}</h2>
          <p className="text-purple-200 text-sm">{t('hotline')}: +8801901634903</p>
        </div>
        {view === 'list' && (
          <button 
            onClick={() => setView('create')}
            className="bg-white text-orange-500 px-6 py-2.5 rounded-lg font-bold hover:bg-purple-50 transition-colors shadow-sm"
          >
            + {t('create_ticket')}
          </button>
        )}
        {view === 'create' && (
          <button 
            onClick={() => setView('list')}
            className="bg-white/20 text-white px-6 py-2.5 rounded-lg font-bold hover:bg-white/30 transition-colors"
          >
            Back to List
          </button>
        )}
      </div>

      {view === 'list' ? (
        <Card>
          <div className="overflow-x-auto">
            <table className="w-full text-left text-sm">
              <thead className="bg-slate-50 border-b">
                <tr>
                  <th className="p-4">{t('ticket_id')}</th>
                  <th className="p-4">{t('date')}</th>
                  <th className="p-4">{t('subject')}</th>
                  <th className="p-4">{t('priority')}</th>
                  <th className="p-4">{t('status')}</th>
                </tr>
              </thead>
              <tbody>
                {tickets.length === 0 ? (
                  <tr><td colSpan="5" className="p-8 text-center text-slate-400">No tickets found. Create one to get support.</td></tr>
                ) : (
                  tickets.map(ticket => (
                    <tr key={ticket.id} className="border-b last:border-0 hover:bg-slate-50 transition-colors">
                      <td className="p-4 font-mono font-medium text-purple-700">{ticket.ticketId}</td>
                      <td className="p-4 text-slate-500">{ticket.date?.toDate ? ticket.date.toDate().toLocaleDateString() : 'Just now'}</td>
                      <td className="p-4 font-medium">{ticket.subject}</td>
                      <td className="p-4">
                        <span className={`px-2 py-1 rounded text-xs font-bold ${
                          ticket.priority === 'High' ? 'bg-red-100 text-red-700' : 
                          ticket.priority === 'Medium' ? 'bg-amber-100 text-amber-700' : 'bg-blue-100 text-blue-700'
                        }`}>
                          {ticket.priority || 'Normal'}
                        </span>
                      </td>
                      <td className="p-4">
                        <span className={`px-2 py-1 rounded text-xs font-bold ${ticket.status==='Closed'?'bg-slate-200 text-slate-600':'bg-emerald-100 text-emerald-700'}`}>
                          {ticket.status}
                        </span>
                      </td>
                    </tr>
                  ))
                )}
              </tbody>
            </table>
          </div>
        </Card>
      ) : (
        <div className="max-w-2xl mx-auto">
          <div className="text-center mb-6">
            <h2 className="text-2xl font-bold text-purple-700">{t('submit_ticket')}</h2>
          </div>
          <Card className="p-8 border-t-4 border-t-purple-600 shadow-lg">
            <div className="space-y-5">
              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">{t('category')}</label>
                <select 
                  className="w-full p-3 border rounded-lg focus:ring-2 focus:ring-purple-500 outline-none bg-slate-50"
                  value={newTicket.category}
                  onChange={e => setNewTicket({...newTicket, category: e.target.value})}
                >
                  <option value="">Select Category</option>
                  <option value="Technical Issue">Technical Issue</option>
                  <option value="Billing">Billing</option>
                  <option value="Feature Request">Feature Request</option>
                  <option value="Other">Other</option>
                </select>
              </div>

              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">{t('priority')}</label>
                <select 
                  className="w-full p-3 border rounded-lg focus:ring-2 focus:ring-purple-500 outline-none bg-slate-50"
                  value={newTicket.priority}
                  onChange={e => setNewTicket({...newTicket, priority: e.target.value})}
                >
                  <option value="">Select Priority</option>
                  <option value="Low">Low</option>
                  <option value="Medium">Medium</option>
                  <option value="High">High</option>
                </select>
              </div>

              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">{t('subject')}</label>
                <input 
                  className="w-full p-3 border rounded-lg focus:ring-2 focus:ring-purple-500 outline-none"
                  placeholder="Type subject"
                  value={newTicket.subject}
                  onChange={e => setNewTicket({...newTicket, subject: e.target.value})}
                />
              </div>

              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">{t('message')}</label>
                <textarea 
                  className="w-full p-3 border rounded-lg focus:ring-2 focus:ring-purple-500 outline-none h-32 resize-none"
                  placeholder="Type your message"
                  value={newTicket.message}
                  onChange={e => setNewTicket({...newTicket, message: e.target.value})}
                ></textarea>
              </div>

              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">{t('upload_files')}</label>
                <div className="border-2 border-dashed border-slate-300 rounded-lg p-8 flex flex-col items-center justify-center text-slate-500 bg-slate-50 hover:bg-slate-100 transition-colors cursor-pointer group">
                  <File className="w-8 h-8 mb-2 text-slate-400 group-hover:text-purple-500 transition-colors"/>
                  <span className="font-medium text-sm">{t('drag_drop')}</span>
                  <span className="text-xs text-slate-400 mt-1">{t('max_size')}</span>
                </div>
              </div>

              <div className="flex justify-end pt-4">
                <button 
                  onClick={handleSubmit}
                  className="bg-purple-600 text-white px-8 py-3 rounded-lg font-bold hover:bg-purple-700 transition-colors shadow-lg shadow-purple-200"
                >
                  {t('submit_btn')}
                </button>
              </div>
            </div>
          </Card>
        </div>
      )}
    </div>
  );
}

// 10. COMMUNITY VIEW
function CommunityView({ lang }) {
  const t = (key) => TRANSLATIONS[lang][key] || key;
  const [copiedLink, setCopiedLink] = useState(null);

  const communities = [
    { name: 'Facebook', url: '', color: 'text-blue-600', bg: 'bg-blue-100', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-blue-600"><path d="M22.675 0h-21.35c-.732 0-1.325.593-1.325 1.325v21.351c0 .731.593 1.324 1.325 1.324h11.495v-9.294h-3.128v-3.622h3.128v-2.671c0-3.1 1.893-4.788 4.659-4.788 1.325 0 2.463.099 2.795.143v3.24l-1.918.001c-1.504 0-1.795.715-1.795 1.763v2.313h3.587l-.467 3.622h-3.12v9.293h6.116c.73 0 1.323-.593 1.323-1.325v-21.35c0-.732-.593-1.325-1.325-1.325z"/></svg>
    )},
    { name: 'Youtube', url: '', color: 'text-red-600', bg: 'bg-red-100', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-red-600"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>
    )},
    { name: 'Instagram', url: '', color: 'text-pink-600', bg: 'bg-pink-100', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-pink-600"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>
    )},
    { name: 'X.com', url: '', color: 'text-slate-900', bg: 'bg-slate-200', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-slate-900"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
    )},
    { name: 'Threads', url: '', color: 'text-slate-900', bg: 'bg-slate-200', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-slate-900"><path d="M12.27 4.15c-4.42 0-7.85 3.32-7.85 7.78 0 4.13 3.19 7.92 7.79 7.92 2.22 0 4.29-1 5.56-2.52l2.03 2.03c-1.85 2.1-4.71 3.49-7.59 3.49-6.33 0-11.21-5.04-11.21-11.05 0-5.83 4.88-10.87 11.27-10.87 6.64 0 10.74 4.92 10.74 10.8v1.1c0 2.21-.52 3.99-2.92 3.99-1.29 0-2.22-.96-2.22-2.73v-2.16c0-3.32-2.06-5.46-4.9-5.46-2.97 0-5.26 2.37-5.26 5.37 0 2.87 2.19 5.23 5.31 5.23 1.94 0 3.39-.81 4.21-2.02v.75c0 3.33 1.13 4.3 3.93 4.3 4.79 0 6.66-4.32 6.66-8.97V11.8c0-7.94-5.59-14.65-15.55-14.65zm-.2 5.28c1.6 0 2.78 1.25 2.78 2.88 0 1.63-1.15 2.91-2.78 2.91-1.67 0-2.85-1.28-2.85-2.91 0-1.63 1.21-2.88 2.85-2.88z"/></svg>
    )},
    { name: 'Tiktok', url: '', color: 'text-slate-900', bg: 'bg-slate-200', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-slate-900"><path d="M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93v6.16c0 2.52-1.12 4.84-2.9 6.24-1.72 1.36-4.03 2.06-6.19 1.37-2.61-.84-4.44-3.1-4.71-5.83-.26-2.7 1.35-5.34 3.74-6.52.57-.27 1.2-.47 1.83-.55v4.04c-.3.03-.61.08-.9.18-1.54.54-2.29 2.37-1.63 3.86.66 1.48 2.33 2.23 3.89 1.73 1.54-.51 2.34-2.29 1.76-3.83-.1-.26-.23-.51-.39-.74v-9.17z"/></svg>
    )},
    { name: 'Linkedin', url: '', color: 'text-blue-700', bg: 'bg-blue-100', icon: (
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-blue-700"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
    )}
  ];

  const handleCopy = (url, name) => {
    if (typeof navigator !== 'undefined' && navigator.clipboard) {
        navigator.clipboard.writeText(url).then(() => {
            setCopiedLink(name);
            setTimeout(() => setCopiedLink(null), 2000);
        }).catch(err => {
            // Fallback for some environments
            const textArea = document.createElement("textarea");
            textArea.value = url;
            document.body.appendChild(textArea);
            textArea.select();
            try {
                document.execCommand('copy');
                setCopiedLink(name);
                setTimeout(() => setCopiedLink(null), 2000);
            } catch (err) {
                console.error('Unable to copy', err);
            }
            document.body.removeChild(textArea);
        });
    }
  };

  return (
    <div className="max-w-7xl mx-auto py-6">
      <h2 className="text-2xl font-bold mb-6 text-slate-900">{t('join_community')}</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        {communities.map((social) => (
          <div key={social.name} className="bg-white rounded-xl p-6 shadow-sm border border-slate-100 flex items-start gap-4 transition-all hover:shadow-md">
            <div className={`p-0 rounded-full flex-shrink-0`}>
               {social.icon}
            </div>
            <div className="flex-1 min-w-0">
              <h3 className="font-bold text-lg text-slate-900 mb-1">{social.name}</h3>
              <p className="text-slate-500 text-sm mb-2">Join Our {social.name} Community <Copy size={12} className="inline ml-1 cursor-pointer hover:text-blue-500" onClick={() => handleCopy(social.url, social.name)}/></p>
              
              <div className="flex items-center gap-2 bg-slate-50 p-2 rounded border border-slate-200">
                 {copiedLink === social.name ? <Check size={14} className="text-emerald-500"/> : <ExternalLink size={14} className="text-slate-400"/>}
                 <a href={social.url} target="_blank" rel="noopener noreferrer" className="text-xs text-slate-600 hover:text-blue-600 truncate flex-1 hover:underline font-mono">
                   {social.url || 'No link added'}
                 </a>
                 <button 
                   onClick={() => handleCopy(social.url, social.name)}
                   className={`text-xs px-2 py-1 rounded font-bold transition-colors ${copiedLink === social.name ? 'bg-emerald-100 text-emerald-700' : 'bg-white border text-slate-600 hover:bg-slate-100'}`}
                 >
                   {copiedLink === social.name ? t('copied') : t('copy_link')}
                 </button>
              </div>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}# SmartBiz-POS
