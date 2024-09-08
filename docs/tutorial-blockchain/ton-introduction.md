---
sidebar_position: 1
---

# Bài 1: Giới thiệu về TON Network và Tương tác Cơ bản

## Tổng quan về TON Network

TON (The Open Network) là một blockchain thế hệ mới, được thiết kế để xử lý hàng triệu giao dịch mỗi giây. Nó được phát triển với mục tiêu tạo ra một hệ sinh thái blockchain hoàn chỉnh, hỗ trợ các ứng dụng phi tập trung (dApps) và các giao thức Web3.

### Các đặc điểm chính của TON:

1. **Khả năng mở rộng cao**: Sử dụng kiến trúc sharding động.
2. **Tốc độ nhanh**: Xử lý giao dịch trong vài giây.
3. **Phí giao dịch thấp**: Phù hợp cho các ứng dụng microtransaction.
4. **Proof-of-Stake**: Cơ chế đồng thuận tiết kiệm năng lượng.
5. **Smart Contracts**: Hỗ trợ ngôn ngữ lập trình Fift và FunC.

## Tương tác cơ bản với TON Network sử dụng JavaScript

Để tương tác với TON Network, chúng ta sẽ sử dụng thư viện `ton` của JavaScript.

### Cài đặt

Trước tiên, hãy cài đặt thư viện TON:

```bash
npm install ton ton-crypto ton-core
```

### Kết nối với TON Network

```javascript
const { TonClient } = require("ton");

const client = new TonClient({
  endpoint: "https://toncenter.com/api/v2/jsonRPC",
});
```

### Đọc số dư của một địa chỉ

```javascript
async function getBalance(address) {
  const balance = await client.getBalance(address);
  console.log(`Số dư của ${address}: ${balance.toString()} nanoTON`);
}

// Sử dụng
getBalance("EQD2NmD_lH5f5u1Kj3KfGyTvhZSX0Y6kc6xn8Z5M_wCdRt-C");
```

### Chuyển tiền

```javascript
const { WalletContractV4 } = require("ton");
const { mnemonicToPrivateKey } = require("ton-crypto");

async function sendTON(from, to, amount, mnemonic) {
  // Chuyển đổi mnemonic thành private key
  const keyPair = await mnemonicToPrivateKey(mnemonic.split(" "));

  // Tạo ví
  const wallet = WalletContractV4.create({ publicKey: keyPair.publicKey, workchain: 0 });

  // Tạo giao dịch
  const transfer = wallet.createTransfer({
    secretKey: keyPair.secretKey,
    toAddress: to,
    amount: amount, // in nanoTON
    seqno: await wallet.getSeqno(),
    bounce: false,
  });

  // Gửi giao dịch
  await client.sendExternalMessage(wallet, transfer);

  console.log(`Đã chuyển ${amount} nanoTON từ ${from} đến ${to}`);
}

// Sử dụng (thay thế bằng thông tin thực tế của bạn)
sendTON(
  "EQD2NmD_lH5f5u1Kj3KfGyTvhZSX0Y6kc6xn8Z5M_wCdRt-C",
  "EQBvW8Z5huBkMJYdnfAEM5JqTNkuWX3diqYENkWsIL0XggGG",
  1000000000, // 1 TON
  "your mnemonic phrase here"
);
```

### Lưu ý về Bảo mật

Khi làm việc với ví và khóa riêng tư, luôn đảm bảo bảo mật thông tin nhạy cảm. Không bao giờ chia sẻ hoặc lưu trữ mnemonic hoặc khóa riêng tư của bạn trong mã nguồn hoặc nơi không an toàn.

## Kết luận

Đây chỉ là những tương tác cơ bản với mạng TON. TON Network còn cung cấp nhiều tính năng mạnh mẽ khác như tương tác với smart contracts, tạo và quản lý tokens, và xây dựng các ứng dụng phi tập trung phức tạp. Khi bạn đã nắm vững những kiến thức cơ bản này, bạn có thể tiếp tục khám phá các tính năng nâng cao hơn của TON.
