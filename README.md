# my-inventory<!DOCTYPE html>
<html lang="lo">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ລະບົບຈັດການສາງສິນຄ້າ</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container-fluid">
            <a class="navbar-brand" href="#"><i class="bi bi-box-seam"></i> ລະບົບສາງສິນຄ້າ</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item"><a class="nav-link active" href="#" onclick="showView('dashboard')"><i class="bi bi-grid-1x2-fill"></i> Dashboard</a></li>
                    <li class="nav-item"><a class="nav-link" href="#" onclick="showView('inventory')"><i class="bi bi-table"></i> ຍອດຄົງເຫຼືອ</a></li>
                    <li class="nav-item"><a class="nav-link" href="#" onclick="showView('receipt')"><i class="bi bi-box-arrow-in-down"></i> ຮັບສິນຄ້າ</a></li>
                    <li class="nav-item"><a class="nav-link" href="#" onclick="showView('issue')"><i class="bi bi-box-arrow-up"></i> ເບີກສິນຄ້າ</a></li>
                </ul>
            </div>
        </div>
    </nav>

    <div class="container mt-4">
        <div id="dashboard-view">
            <h3><i class="bi bi-grid-1x2-fill"></i> Dashboard</h3>
            <div class="row g-3 mb-4">
                <div class="col-md-3"><div class="card text-center p-3 shadow-sm"><h5 class="card-title">ສິນຄ້າທັງໝົດ</h5><p class="card-text fs-4" id="total-products">0</p></div></div>
                <div class="col-md-3"><div class="card text-center p-3 shadow-sm"><h5 class="card-title">ມູນຄ່າສິນຄ້າ (ກີບ)</h5><p class="card-text fs-4" id="total-value">0</p></div></div>
                <div class="col-md-3"><div class="card text-center p-3 shadow-sm bg-warning"><h5 class="card-title">ສິນຄ້າໃກ້ໝົດ</h5><p class="card-text fs-4" id="low-stock-products">0</p></div></div>
                <div class="col-md-3"><div class="card text-center p-3 shadow-sm bg-danger text-white"><h5 class="card-title">ສິນຄ້າໝົດແລ້ວ</h5><p class="card-text fs-4" id="out-of-stock-products">0</p></div></div>
            </div>
            
            <h5><i class="bi bi-activity"></i> ການເຄື່ອນໄຫວລ່າສຸດ</h5>
            <div class="table-responsive">
                <table class="table table-striped table-hover">
                    <thead><tr><th>ວັນທີ</th><th>ປະເພດ</th><th>ເລກທີເອກະສານ</th><th>ລາຍລະອຽດ</th></tr></thead>
                    <tbody id="recent-activity-table">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="inventory-view" class="view-hidden">
            <div class="d-flex justify-content-between align-items-center mb-3">
                <h3><i class="bi bi-table"></i> ລາຍງານ ແລະ ຍອດຄົງເຫຼືອ</h3>
                <div>
                     <button class="btn btn-success" onclick="exportToExcel()"><i class="bi bi-file-earmark-excel"></i> Export to Excel</button>
                </div>
            </div>
            <div class="row mb-3">
                <div class="col-md-6">
                    <input type="text" id="inventory-search" class="form-control" placeholder="ຄົ້ນຫາຕາມຊື່ ຫຼື ລະຫັດສິນຄ້າ...">
                </div>
                <div class="col-md-4">
                    <select id="inventory-filter-status" class="form-select">
                        <option value="all">ສະຖານະທັງໝົດ</option>
                        <option value="in_stock">ມີສິນຄ້າ</option>
                        <option value="low_stock">ໃກ້ໝົດ</option>
                        <option value="out_of_stock">ໝົດແລ້ວ</option>
                    </select>
                </div>
            </div>
            <div class="table-responsive">
                <table class="table table-bordered table-hover" id="inventory-table-export">
                    <thead class="table-dark">
                        <tr>
                            <th>ລະຫັດສິນຄ້າ</th>
                            <th>ຊື່ສິນຄ້າ</th>
                            <th>ຮັບເຂົ້າທັງໝົດ</th>
                            <th>ເບີກອອກທັງໝົດ</th>
                            <th>ຍອດຄົງເຫຼືອ</th>
                            <th>ສະຖານະ</th>
                        </tr>
                    </thead>
                    <tbody id="inventory-table-body">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="receipt-view" class="view-hidden">
            <h3><i class="bi bi-box-arrow-in-down"></i> ບັນທຶກການຮັບສິນຄ້າ</h3>
            <form id="receipt-form" class="card p-4 shadow-sm">
                <div class="row g-3">
                    <div class="col-md-4">
                        <label class="form-label">ເລກທີເອກະສານ</label>
                        <input type="text" id="receipt-doc-no" class="form-control" readonly>
                    </div>
                    <div class="col-md-4">
                        <label class="form-label">ວັນທີຮັບ</label>
                        <input type="date" id="receipt-date" class="form-control" required>
                    </div>
                     <div class="col-md-4">
                        <label class="form-label">ເລກທີໃບສັ່ງຊື້ (PO No.)</label>
                        <input type="text" class="form-control" placeholder="PO-12345">
                    </div>
                </div>
                <hr>
                <h5>ລາຍການສິນຄ້າ</h5>
                <div class="table-responsive">
                    <table class="table">
                        <thead><tr><th>ລະຫັດສິນຄ້າ (ຄົ້ນຫາ)</th><th>ຈຳນວນ</th><th>ໝາຍເຫດ</th></tr></thead>
                        <tbody id="receipt-items">
                           <tr>
                               <td><input type="text" class="form-control product-search" placeholder="ພິມລະຫັດ ຫຼື ຊື່ສິນຄ້າ"></td>
                               <td><input type="number" class="form-control" min="1" value="1"></td>
                               <td><input type="text" class="form-control" placeholder="ສິນຄ້າສະພາບດີ"></td>
                           </tr>
                        </tbody>
                    </table>
                </div>
                <div class="d-flex justify-content-end">
                    <button type="submit" class="btn btn-primary"><i class="bi bi-save"></i> ບັນທຶກການຮັບສິນຄ້າ</button>
                </div>
            </form>
        </div>
        
        <div id="issue-view" class="view-hidden">
            <h3><i class="bi bi-box-arrow-up"></i> ສ້າງໃບເບີກສິນຄ້າ</h3>
            <form id="issue-form" class="card p-4 shadow-sm">
                 <div class="row g-3">
                    <div class="col-md-4">
                        <label class="form-label">ວັນທີເບີກ</label>
                        <input type="date" id="issue-date" class="form-control" required>
                    </div>
                    <div class="col-md-4">
                        <label class="form-label">ຊື່ຜູ້ເບີກ</label>
                        <input type="text" class="form-control" id="issuer-name" placeholder="ປ້ອນຊື່ຜູ້ເບີກ" required>
                    </div>
                    <div class="col-md-4">
                        <label class="form-label">ເຫດຜົນການເບີກ</label>
                        <input type="text" class="form-control" id="issue-reason" placeholder="ໃຊ້ສຳລັບໂຄງການ...">
                    </div>
                </div>
                <hr>
                <h5>ລາຍການເບີກ</h5>
                 <div class="table-responsive">
                    <table class="table">
                        <thead><tr><th>ລະຫັດສິນຄ້າ (ຄົ້ນຫາ)</th><th>ຍອດເຫຼືອ</th><th>ຈຳນວນເບີກ</th></tr></thead>
                        <tbody id="issue-items">
                           <tr>
                               <td><input type="text" class="form-control product-search" placeholder="ພິມລະຫັດ ຫຼື ຊື່ສິນຄ້າ"></td>
                               <td><input type="text" class="form-control" readonly value="0"></td>
                               <td><input type="number" class="form-control" min="1" value="1"></td>
                           </tr>
                        </tbody>
                    </table>
                </div>
                <div class="d-flex justify-content-end gap-2">
                    <button type="submit" class="btn btn-primary"><i class="bi bi-save"></i> ບັນທຶກ</button>
                    <button type="button" id="save-and-print-btn" class="btn btn-info"><i class="bi bi-printer"></i> ບັນທຶກ ແລະ ປີ້ນໃບເບີກ</button>
                </div>
            </form>
        </div>
    </div>
    
    <div id="print-area" class="print-only">
        <div class="print-header">
            <h2>ໃບເບີກສິນຄ້າ (Goods Issue Slip)</h2>
            <hr>
        </div>
        <div class="print-details">
            <p><strong>ເລກທີໃບເບີກ:</strong> <span id="print-gi-no"></span></p>
            <p><strong>ວັນທີເບີກ:</strong> <span id="print-issue-date"></span></p>
            <p><strong>ຜູ້ເບີກ:</strong> <span id="print-issuer-name"></span></p>
            <p><strong>ເຫດຜົນ:</strong> <span id="print-issue-reason"></span></p>
        </div>
        <table class="print-table">
            <thead>
                <tr>
                    <th>ລ/ດ</th>
                    <th>ລະຫັດສິນຄ້າ</th>
                    <th>ລາຍການ</th>
                    <th>ຈຳນວນ</th>
                </tr>
            </thead>
            <tbody id="print-table-body">
            </tbody>
        </table>
        <div class="print-signature">
            <div class="sig-box">
                <p>...............................</p>
                <p>(<span id="sig-issuer-name"></span>)</p>
                <p><strong>ຜູ້ເບີກ (Issuer)</strong></p>
            </div>
            <div class="sig-box">
                <p>...............................</p>
                <p>(...............................)</p>
                <p><strong>ຜູ້ກວດສອບ (Checker)</strong></p>
            </div>
            <div class="sig-box">
                <p>...............................</p>
                <p>(...............................)</p>
                <p><strong>ຜູ້ອະນຸມັດ (Approver)</strong></p>
            </div>
        </div>
    </div>


    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://unpkg.com/xlsx/dist/xlsx.full.min.js"></script>
    <script src="app.js"></script>
</body>
</html>-app
