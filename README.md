# pagination_php_procedural
A simple pagination made with php procedural programming. It's beginner level.
Made with bulma css framework AND PHP.



    <!--Getting number of page from get and storing -->
    <?php $page =  (isset($_GET['page'] ) && $_GET['page'] > 0) ?  (int)$_GET['page'] : 0;

    //  At just index.php,  
        if (!isset($_GET['page'])) {
            $page = 0;
        }

    ?>


    <?php
    // Max posts allowed on per page
        $perpage = 10;


        if ($page >= 1) {
            $limit = ($perpage * $page ) - $perpage  ;
        } else {
            $limit = 0;
        }
    ?>


      <?php

      // to get total of no of records for pagination
      $query = "SELECT * FROM posts ;";

      $result = result($query);

      check_query($result);

      $maxRows = mysqli_num_rows($result);



      // Query to get posts for main page
      $query = "SELECT * FROM posts ORDER BY post_id DESC LIMIT $limit,$perpage;";

      $result = mysqli_query($connection, $query);


      if (!$result) {
          echo "Error at result". $query. mysqli_error($connection);
      }

      $no_of_result = mysqli_num_rows($result);

      // Two queries because one is solly used to get max number of tables inside db and other queries number of rows
      // changes as page changes
      ?>

    <?php
      // total number of pagination is ($maxRows)max number of rows by $perpage
          $total = ceil($maxRows / $perpage );

      ?>
    

      // The part that out puts the html for posts (change get_body as to your needs since )

              <?php
          if ($no_of_result > 0) {
          while ($row = mysqli_fetch_array($result)) { // have posts

              get_body($row);
              }
              } else { // no posts and last page
                  echo "<h3 class='title is-3 has-text-centered'> Nothing Found <h3/>" ;
              }
              mysqli_free_result($result);
      ?>



      <?php

      // $total = total numbers of rows being fetched from database
      // $page = $_GET['page'] or page number getting from get
      // $n = variable intiated by for loop

      // second time assigning total because its easy to test pagination via $total (you can remove if you want)
      $total = $total;

      ?>


      <!--Pagination start-->
      <div class="columns is-mobile is-centered">
      <!--Div to center allign pagination start-->
          <div class="pagination is-centered" >

      <!--        Will create pagination if total no of row is greater then 1-->
              <?php if ($total >= 1) :?>

                  <nav class="pagination is-centered" role="navigation" aria-label="pagination">

                      <?php if ($page === 0) :?>
      <!-- Disable to go to left page button if $page is equal to 0 or first page-->
                      <a disabled="disabled" class="pagination-previous" href="index.php?page=<?php echo $page - 1?>"> &#60;&#60; </a>
                      <?php else:?>
      <!--                Else it is left page button is enabled-->
       <a class="pagination-previous" href="index.php?page=<?php echo $page - 1?>"> &#60;&#60; </a>

                  <?php endif;?>
      <!--Main paginattion container-->
                      <ul class="pagination-list">

                          <?php
      // Will page page link no 0 active with class is-current
                          if ($page == 0) :
                          ?>
                              <li><a class="pagination-link is-current" href="index.php?page=0" aria-label="Goto page <?php echo '0'?>"><?php echo 0?></a></li>
                          <?php
                          else:
                              ?>
      <!--                    If it's not current page then -->
                              <li><a class="pagination-link" href="index.php?page=0" aria-label="Goto page <?php echo '0'?>"><?php echo 0?></a></li>

                          <?php
                          endif;
                          ?>
                          <?php

                          //  start for loop
                          for ($n = 1; $n <= $total; $n++) : ?>

      <!--                        --><?php //if ($n == $total ) :?>
      <!--                            --><?php //continue  ?>
      <!--                        --><?php //endif;?>


      <!--                    add class if $n is current page -->
                              <?php if ($n === $page) : ?>

      <!-- Will make current page active with class is-current if its current page -->
                                  <li><a class="pagination-link is-current" href="index.php?page=<?php echo $n ?>" aria-label="Page <?php echo $n ?>" aria-current="page"><?php echo $n?></a></li>

                              <?php else:?>

                                  <!--                        Start ellipses after page number 0 -->
                                  <?php if ($n == 1 && $total > 6 && $page > 3) :?>

                                      <li><span class="pagination-ellipsis">&hellip;</span></li>

                          <?php endif; ?>
      <!--                        End ellipses after page number 0 -->

      <!--                        Start ellipses at total no of maxpage minus 1-->
                                      <?php if ($n == ($page + 4) && $total > 6 && $page < ($total - 2 ) ) :?>

                                      <li><span class="pagination-ellipsis">&hellip;</span></li>

                                  <?php endif; ?>
                                  <!--                        End ellipses at total no of maxpage minus 1-->


      <!--        Main body of pagination-->
                                  <?php if ($n > ($page - 3) && $n < ($page + 3) ):?>
                                  <li><a class="pagination-link" href="index.php?page=<?php echo $n ?>" aria-label="Goto page <?php echo $n?>"><?php echo $n?></a></li>
                                  <?php else: ?>
                                      <?php continue;?>
                          <?php endif; ?>

      <!--                        End main pagination -->

                              <?php endif; ?>

                          <?php endfor; ?>
      <!--End foreach-->

      <!--                    Start last page button -->

      <!--                    Will not show last page button at some conditions -->
                          <?php if ($page !== ($total - 1) || $page !== ($total - 2) )  :?>

                              <?php else:?>
      <!--                    Will show last normal page button-->
                              <li><a class="pagination-link" href="index.php?page=<?php echo $total?>"  aria-label="Goto page <?php echo $total?>"><?php echo $total?></a></li>
                          <?php endif;?>

      <!--If you remove this then pagination will not show one page button ( $total - 2 )-->
                          <?php if ($page < ($total - 2 ) ) :?>
                              <li><a class="pagination-link" href="index.php?page=<?php echo $total?>"  aria-label="Goto page <?php echo $total?>"><?php echo $total?></a></li>


                          <?php endif;?>
      <!--                    End last page button -->


                      </ul>
                      <!--Main paginattion container end-->

      <!-- Next page start-->
                      <?php if ($page === $total) :?>
                            <a class="pagination-next" disabled="disabled" href="index.php?page=<?php echo $page + 1?>">  	&#62;&#62; </a>

                      <?php else:?>
                      <a class="pagination-next" href="index.php?page=<?php echo $page + 1?>">  	&#62;&#62; </a>
                      <?php endif;?>
                  </nav>
              <?php endif;?>
      <!--        Next page end-->

          </div>
          <!--Div to center allign pagination end-->

      </div>
      <!--Pagination end -->
