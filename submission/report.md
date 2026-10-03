# Báo cáo Lab 16 — Benchmark LightGBM trên GCP CPU

1. Tôi dùng GCP (Google Cloud Platform), region us-central1 / zone us-central1-a, instance type e2-medium (2 vCPU, 4GB RAM), source commit 55539f6.
2. Dataset có 284,807 dòng (492 dòng gian lận ~ 0.17%), chia train/validation/test theo tỷ lệ 60% / 20% / 20% (170,883 / 56,962 / 56,962 dòng, stratified theo nhãn Class), seed 16.
3. Load dữ liệu mất 2.63 giây; training mất 4.24 giây; best iteration là 68 (early stopping sau 20 vòng).
4. AUC 0.9768, Accuracy 0.9995 (99.95%), F1 0.8478, Precision 0.9070 (90.70%), Recall 0.7959 (79.59%) trên tập test.
5. Latency 1 dòng 1.34 ms; throughput batch 1.000 dòng 244,558 dòng/giây; cách đo lấy median qua 50 lần đo với 1 dòng và 10 lần đo với batch 1.000 dòng, loại trừ lần chạy warm-up đầu tiên, dùng predict_proba trên pandas DataFrame.
6. CPU/RAM/Network tôi quan sát lúc benchmark đang chạy CPU đạt 198.7% (tận dụng trọn vẹn 2 vCPU), sau khi chạy xong RAM đã dùng 490Mi / 3.8Gi, Network nhận 259.7 MB (27,472 packets) và gửi 3.1 MB (17,851 packets); ảnh đính kèm trong thư mục screenshots/ (top.png, free.png, network.png).
7. Billing tại Google Cloud Console (ghi nhận tích lũy ngày 02/10 và 03/10/2026) ghi nhận 39,198 VNĐ (gồm Compute Engine 21,236 VNĐ và Networking 17,963 VNĐ); ảnh đính kèm screenshots/billing.png.
8. Tôi đã tải kết quả và xóa tài nguyên lúc 23:35 ngày 03/10/2026; bằng chứng dọn dẹp là lệnh terraform destroy đã hoàn tất thành công, kiểm tra lại bằng gcloud compute instances list, routers list, addresses list đều trả về "Listed 0 items".
