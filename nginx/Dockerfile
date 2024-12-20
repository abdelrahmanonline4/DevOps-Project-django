# استخدام صورة Nginx الرسمية
FROM nginx:1.23

# حذف ملف الإعدادات الافتراضي (اختياري للتأكد من عدم وجود تعارضات)
RUN rm /etc/nginx/conf.d/default.conf

# نسخ ملف الإعدادات الخاص بك إلى الحاوية
COPY nginx.conf /etc/nginx/conf.d/default.conf

# تعيين حقوق الوصول (اختياري)
RUN chmod 644 /etc/nginx/conf.d/default.conf
# فتح منفذ 80
EXPOSE 80

